#### BlazorHotelBooking/Client/Auth/APIAuthStateProvider.cs

```c#
using Blazored.LocalStorage;
using Microsoft.AspNetCore.Components.Authorization;
using System.Net.Http.Headers;
using System.Security.Claims;
using System.Text.Json;

namespace BlazorHotelBooking.Client.Auth
{
    public class APIAuthStateProvider : AuthenticationStateProvider
    {
        private readonly HttpClient _httpClient;
        private readonly ILocalStorageService _localStorage;

        public APIAuthStateProvider(HttpClient httpClient, ILocalStorageService localStorage)
        {
            _httpClient = httpClient;
            _localStorage = localStorage;
        }

        public override async Task<AuthenticationState> GetAuthenticationStateAsync()
        {
            string userToken = await _localStorage.GetItemAsync<string>("authToken");

            if (string.IsNullOrWhiteSpace(userToken))
            {
                return new AuthenticationState(new ClaimsPrincipal(new ClaimsIdentity()));
            }

            _httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", userToken);

            return new AuthenticationState(new ClaimsPrincipal(new ClaimsIdentity(ParseClaimsFromJwt(userToken), "jwt")));
        }

        public void MarkUserAsAuthenticated(string token)
        {
            ClaimsPrincipal authenticatedUser = new ClaimsPrincipal(new ClaimsIdentity(ParseClaimsFromJwt(token), "jwt"));
            Task<AuthenticationState> authState = Task.FromResult(new AuthenticationState(authenticatedUser));
            NotifyAuthenticationStateChanged(authState);
        }

        public void MarkUserAsLoggedOut()
        {
            ClaimsPrincipal anonymousUser = new ClaimsPrincipal(new ClaimsIdentity());
            Task<AuthenticationState> authState = Task.FromResult(new AuthenticationState(anonymousUser));
            NotifyAuthenticationStateChanged(authState);
        }

        private IEnumerable<Claim> ParseClaimsFromJwt(string jwt)
        {
            List<Claim> claims = new List<Claim>();
            string payload = jwt.Split('.')[1];
            byte[] jsonBytes = ParseBase64WithoutPadding(payload);
            Dictionary<string, object>? keyValuePairs = JsonSerializer.Deserialize<Dictionary<string, object>>(jsonBytes);

            keyValuePairs!.TryGetValue(ClaimTypes.Role, out object roles);

            if (roles != null)
            {
                if (roles.ToString()!.Trim().StartsWith("["))
                {
                    string[]? parsedRoles = JsonSerializer.Deserialize<string[]>(roles.ToString()!);

                    foreach (string parsedRole in parsedRoles!)
                    {
                        claims.Add(new Claim(ClaimTypes.Role, parsedRole));
                    }
                }
                else
                {
                    claims.Add(new Claim(ClaimTypes.Role, roles.ToString()!));
                }

                keyValuePairs.Remove(ClaimTypes.Role);
            }

            claims.AddRange(keyValuePairs.Select(kvp => new Claim(kvp.Key, kvp.Value.ToString()!)));

            return claims;
        }

        private byte[] ParseBase64WithoutPadding(string base64)
        {
            switch (base64.Length % 4)
            {
                case 2: base64 += "=="; break;
                case 3: base64 += "="; break;
            }

            return Convert.FromBase64String(base64);
        }
    }
}
```

#### BlazorHotelBooking/Client/Pages/Admin.razor
```c#
@page "/admin"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize(Roles = "Admin")]

<h3>Admin Page</h3>

<button @onclick="ViewPayments" class="btn btn-success">View All Payments</button>
<button @onclick="ViewBookings" class="btn btn-success">View All Bookings</button>

<br />
<br />

<button @onclick="AddHotel" class="btn btn-primary">Add Hotel</button>
<br />

@if (hotels is null)
{
    <span>Loading Hotels…</span>
}
else
{
    <h5>Hotels</h5>

    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Single Bed</th>
                <th>Double Bed</th>
                <th>Family Room</th>
            </tr>

        </thead>
        <tbody>
            @foreach (var h in hotels)
            {
                <tr>
                    <td width="5%">@h.Id</td>
                    <td width="5%">@h.Name</td>
                    <td width="5%">@h.SBPrice</td>
                    <td width="5%">@h.DBPrice</td>
                    <td width="5%">@h.FamPrice</td>
                    <td width="5%">
                        <button @onclick="(() => EditHotel(h.Id))" class="btn btn-primary">Edit</button>
                    </td>
                </tr>

            }
        </tbody>
    </table>
}

<br/>
<button @onclick="AddTour" class="btn btn-primary">Add Tour</button>
<br />
@if (tours is null)
{
    <span>Loading Tours…</span>
}
else
{
    <h5>Tours</h5>
    <table class="table">
        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Price</th>
                <th>Duration</th>
                <th>Max Num Of Guests</th>
            </tr>

        </thead>
        <tbody>
            @foreach (var t in tours)
            {
                <tr>
                    <td width="5%">@t.Id</td>
                    <td width="5%">@t.Name</td>
                    <td width="5%">@t.Cost</td>
                    <td width="5%">@t.DurationInDays</td>
                    <td width="5%">@t.MaxNumberOfGuests</td>
                    <td width="5%">
                        <button @onclick="(() => EditTour(t.Id))" class="btn btn-primary">Edit</button>
                    </td>
                </tr>

            }
        </tbody>
    </table>
}



@code {
    List<Hotel>? hotels;
    List<Tour>? tours;

    protected override async Task OnInitializedAsync()
    {
        var result = await http.GetFromJsonAsync<List<Hotel>>("/api/hotel");
        if (result != null)
        {
            hotels = result;
        }

        var result2 = await http.GetFromJsonAsync<List<Tour>>("/api/tour");
        if (result2 != null)
        {
            tours = result2;
        }   


    }

    private void AddHotel()
    {
        NavigationManager.NavigateTo("/hoteledit");
    }

    private void EditHotel(int id)
    {
        NavigationManager.NavigateTo($"/hoteledit/{id}");
    }

    private void AddTour()
    {
        NavigationManager.NavigateTo("/touredit");
    }

    private void EditTour(int id)
    {
        NavigationManager.NavigateTo($"/touredit/{id}");
    }

    private void ViewPayments()
    {
        NavigationManager.NavigateTo("/admin/payments");
    }

    private void ViewBookings()
    {
        NavigationManager.NavigateTo("/admin/bookings");
    }
}
```

#### BlazorHotelBooking/Client/Pages/AdminBookings.razor
```c#
@page "/admin/bookings"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@using System.Security.Claims
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize(Roles ="Admin")]


@if (hotel is null)
{
    <span>Loading hotels…</span>
}
else
{
    <h5>Hotel Bookings</h5>

    <table class="table">
        <thead>
            <tr>
                <th>User ID</th>
                <th>Booking ID</th>
                <th>Hotel ID</th>
                <th>Room Type</th>
                <th>Check in</th>
                <th>Check Out</th>
                <th>Nights</th>
                <th>Deposit</th>
                <th>Total Price</th>
                <th>Paid In Full</th>
                <th>Cancelled</th>
            </tr>

        </thead>
        <tbody>
            @foreach (var h in hotel)
            {
                <tr>
                    <td width="5%">@h.UserId</td>
                    <td width="5%">@h.Id</td>
                    <td width="5%">@h.HotelId</td>
                    <td width="5%">@h.RoomType</td>
                    <td width="5%">@h.CheckIn</td>
                    <td width="5%">@h.CheckOut</td>
                    <td width="5%">@h.NumberOfNights</td>
                    <td width="5%">@h.DepositAmountPaid</td>
                    <td width="5%">@h.TotalPrice</td>
                    <td width="5%">@h.PaidInfull</td>
                    <td width="5%">@h.IsCancelled</td>
                </tr>
            }
        </tbody>
    </table>
}

@if (tour is null)
{
    <span>Loading tours…</span>
}
else
{
    <h5>Tour Bookings</h5>

    <table class="table">
        <thead>
            <tr>
                <th>User ID</th>
                <th>Booking ID</th>
                <th>Tour ID</th>
                <th>Start Date</th>
                <th>End Date</th>
                <th>Num Of People</th>
                <th>Deposit</th>
                <th>Total Price</th>
                <th>Paid In Full</th>
                <th>Cancelled</th>
            </tr>

        </thead>
        <tbody>
            @foreach (var t in tour)
            {
                <tr>
                    <td width="5%">@t.UserId</td>
                    <td width="5%">@t.Id</td>
                    <td width="5%">@t.TourId</td>
                    <td width="5%">@t.CommencementDate</td>
                    <td width="5%">@t.EndDate</td>
                    <td width="5%">@t.NumberOfPeople</td>
                    <td width="5%">@t.DepositAmountPaid</td>
                    <td width="5%">@t.TotalPrice</td>
                    <td width="5%">@t.PaidInfull</td>
                    <td width="5%">@t.IsCancelled</td>
                </tr>
            }
        </tbody>
    </table>
}

@if (package is null)
{
    <span>Loading packages…</span>
}
else
{
    <h5>Package Bookings</h5>

    <table class="table">
        <thead>
            <tr>
                <th>User ID</th>
                <th>Booking ID</th>
                <th>Tour ID</th>
                <th>Start Date</th>
                <th>End Date</th>
                <th>Num Of People</th>
                <th>Hotel ID</th>
                <th>Room Type</th>
                <th>Check in</th>
                <th>Check Out</th>
                <th>Nights</th>
                <th>Deposit</th>
                <th>Total Price</th>
                <th>Paid In Full</th>
                <th>Cancelled</th>
            </tr>

        </thead>
        <tbody>
            @foreach (var p in package)
            {
                <tr>
                    <td width="5%">@p.UserId</td>
                    <td width="5%">@p.Id</td>
                    <td width="5%">@p.TourId</td>
                    <td width="5%">@p.TourStartDate</td>
                    <td width="5%">@p.TourEndDate</td>
                    <td width="5%">@p.NumberOfPeopleOnTour</td>
                    <td width="5%">@p.HotelId</td>
                    <td width="5%">@p.RoomType</td>
                    <td width="5%">@p.HotelCheckIn</td>
                    <td width="5%">@p.HotelCheckOut</td>
                    <td width="5%">@p.NumberOfNights</td>
                    <td width="5%">@p.DepositAmountPaid</td>
                    <td width="5%">@p.TotalPrice</td>
                    <td width="5%">@p.PaidInfull</td>
                    <td width="5%">@p.IsCancelled</td>
                </tr>
            }
        </tbody>
    </table>
}


@code {
    List<HotelBooking> hotel = new List<HotelBooking>();
    List<TourBooking> tour = new List<TourBooking>();
    List<PackageBooking> package = new List<PackageBooking>();

    [CascadingParameter]
    private Task<AuthenticationState>? authenticationState { get; set; }

    protected override async Task OnInitializedAsync()
    {
        var result = await http.GetFromJsonAsync<List<HotelBooking>>($"/api/Admin/hotelbooking");
        if (result != null)
        {
            hotel = result;
        }

        var result2 = await http.GetFromJsonAsync<List<TourBooking>>($"/api/Admin/tourbooking");
        if (result2 != null)
        {
            tour = result2;
        }

        var result3 = await http.GetFromJsonAsync<List<PackageBooking>>($"/api/Admin/packagebooking");
        if (result3 != null)
        {
            package = result3;
        }

    }  
}
```

#### BlazorHotelBooking/Client/Pages/AdminPayments.razor
```c#
@page "/admin/payments"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@using System.Security.Claims
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize(Roles ="Admin")]


@if (payments is null)
{
    <span>Loading Payments…</span>
}
else
{
    <h5>Payments</h5>

    <table class="table">
        <thead>
            <tr>
                <th>User ID</th>
                <th>Booking ID</th>
                <th>Booking Type</th>
                <th>Payment Type</th>
                <th>Payment Date</th>
                <th>Amount Paid</th>
            </tr>

        </thead>
        <tbody>
            @foreach (var p in payments)
            {
                <tr>
                    <td width="5%">@p.UserId</td>
                    <td width="5%">@p.bookingId</td>
                    <td width="5%">@p.bookingType</td>
                    <td width="5%">@p.paymentType</td>
                    <td width="5%">@p.PaymentDate</td>
                    <td width="5%">@p.AmountPaid</td>
                </tr>
            }
        </tbody>
    </table>
}





@code {
    List<Payments> payments = new List<Payments>();

    [CascadingParameter]
    private Task<AuthenticationState>? authenticationState { get; set; }

    protected override async Task OnInitializedAsync()
    {
        var result = await http.GetFromJsonAsync<List<Payments>>($"/api/Payment");
        if (result != null)
        {
            payments = result;
        }
    }
}
```
