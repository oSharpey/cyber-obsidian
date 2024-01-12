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

####  BlazorHotelBooking/Client/Pages/EditHotelBooking.razor
```c#
@page "/editbooking/hotel/{id}"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize]


<PageTitle> Edit Booking </PageTitle>
<h4>Editing booking for @hotel.Name on @selectedBooking.CheckIn.ToString("dd/MM/yyyy")</h4>


<EditForm Model="selectedBooking" OnSubmit="HandleSubmit">
    <div>
        <label for="HotelName">Hotel</label>
        <InputText id="HotelName" @bind-Value="hotel.Name" class="form-control" disabled="true" />
    </div>
    <div>
        <label for="RoomType">Room Type</label>
        <InputSelect id="roomType" @bind-Value="selectedBooking.RoomType" placeholder="Room Type">
            <option value="">---</option>
            @foreach (var room in roomType)
            {
                <option value="@room">@room</option>
            }
        </InputSelect>
    </div>
    <div>
        <label for="NumOfNights">Number Of Nights</label>
        <InputNumber id="NumOfNights" @bind-Value="selectedBooking.NumberOfNights" class="form-control" />
    </div>
    @{
        var currentDeposit = selectedBooking.DepositAmountPaid;
        decimal currentTotal = selectedBooking.TotalPrice;
        var currentCheckOut = selectedBooking.CheckOut;

        switch (selectedBooking.RoomType)
        {
            case "Single":
                newTotal = hotel.SBPrice * selectedBooking.NumberOfNights;
                newDeposit = newTotal / 5;
                newCheckOut = selectedBooking.CheckIn.AddDays(selectedBooking.NumberOfNights);
                break;
            case "Double":
                newTotal = hotel.DBPrice * selectedBooking.NumberOfNights;
                newDeposit = newTotal / 5;
                newCheckOut = selectedBooking.CheckIn.AddDays(selectedBooking.NumberOfNights);
                break;
            case "Family":
                newTotal = hotel.FamPrice * selectedBooking.NumberOfNights;
                newDeposit = newTotal / 5;
                newCheckOut = selectedBooking.CheckIn.AddDays(selectedBooking.NumberOfNights);
                break;
        }


        if(selectedBooking.PaidInfull)
        {
            if (newTotal > currentTotal)
            {
                difference = newTotal - currentTotal;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>You will need to pay £@difference more to complete this booking</p>
                </div>
            }
            else if (newTotal < currentTotal)
            {
                var refund = currentTotal - newTotal;
                difference = refund * -1;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>You will be refunded £@refund</p>
                </div>
            }
        }
        else
        {
            if (newTotal > currentTotal)
            {
                difference = newDeposit - currentDeposit;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>The new deposit for this booking is £@newDeposit</p>
                    <p>You will need to pay £@difference more to complete this booking</p>
                </div>

            }
            else if (newTotal < currentTotal)
            {
                var refund = currentDeposit - newDeposit;
                difference = refund * -1;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>The new deposit for this booking is £@newDeposit</p>
                    <p>You will be refunded £@refund</p>
                </div>

            }
        }

        surcharge = newTotal * (decimal)0.05;

        <p>Modifying this booking will also incure a 5% surcharge (£@surcharge)</p>

        if (showOverlap)
        {
            <div class="alert alert-danger" role="alert">
                <p>There are no more spaces for this room on these dates please select new dates.</p>
            </div>
        }
    }
    <button type="submit" class="btn btn-primary">Save</button>
</EditForm> 


@code {
    [Parameter]
    public string? Id { get; set; }

    private HotelBooking selectedBooking = new HotelBooking();
    private Hotel hotel = new Hotel();
    List<string> roomType = new List<string>() { "Single", "Double", "Family" };
    private int currentNights;
    private decimal newTotal, newDeposit;
    private DateTime newCheckOut;
    private bool showOverlap = false;
    private int numOfOverlap;
    private decimal difference;
    private decimal surcharge;

    protected override async Task OnParametersSetAsync()
    {
        if (Id is not null)
        {
            var result = await http.GetFromJsonAsync<HotelBooking>($"api/bookings/hotel/{Id}");
            if (result is not null)
            {
                selectedBooking = result;
            }
        }

        currentNights = selectedBooking.NumberOfNights;

        hotel = await http.GetFromJsonAsync<Hotel>($"api/hotel/{selectedBooking.HotelId}");
    }

    async Task HandleSubmit()
    {

        if (selectedBooking.PaidInfull)
        {
            selectedBooking.DepositAmountPaid = newDeposit;
            selectedBooking.TotalPrice = newTotal;
        }
        else
        {
            selectedBooking.DepositAmountPaid = newDeposit;
            selectedBooking.TotalPrice = newTotal;
        }
        
        selectedBooking.CheckOut = newCheckOut;

        numOfOverlap = await http.GetFromJsonAsync<int>($"/api/bookings/hotel/overlap?checkIn={selectedBooking.CheckIn}&checkOut={selectedBooking.CheckOut}&hotelId={selectedBooking.HotelId}&roomType={selectedBooking.RoomType}");

        if (numOfOverlap > 20)
        {
            showOverlap = true;
        }
        else
        {
            await http.PutAsJsonAsync($"api/bookings/hotel/{Id}?paymentRemainder={difference}&surcharge={surcharge}", selectedBooking);
            NavigationManager.NavigateTo("/mybookings");
        }
    }
}
```


#### BlazorHotelBooking/Client/Pages/EditPackageBooking.razor
```c#
@page "/editbooking/package/{id}"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize]


<PageTitle> Edit Booking </PageTitle>
<h7>Editing booking for @hotel.Name on @selectedBooking.HotelCheckIn.ToString("dd/MM/yyyy") & @tour.Name on @selectedBooking.TourStartDate</h7>


<EditForm Model="selectedBooking" OnSubmit="HandleSubmit">
    <div>
        <label for="HotelName">Hotel</label>
        <InputText id="HotelName" @bind-Value="hotel.Name" class="form-control" disabled="true" />
    </div>
    <div>
        <label for="RoomType">Room Type</label>
        <InputSelect id="roomType" @bind-Value="selectedBooking.RoomType" placeholder="Room Type">
            <option value="">---</option>
            @foreach (var room in roomType)
            {
                <option value="@room">@room</option>
            }
        </InputSelect>
    </div>
    <div>
        <label for="NumOfNights">Number Of Nights</label>
        <InputNumber id="NumOfNights" @bind-Value="selectedBooking.NumberOfNights" class="form-control" />
    </div>
    <br/>
    <br/>
    <div>
        <label for="TourName">Tour</label>
        <InputText id="TourName" @bind-Value="tour.Name" class="form-control" disabled="true" />
    </div>
    <div>
        <label for="NumOfPeople">Number Of Guests</label>
        <InputNumber id="NumOfPeople" @bind-Value="selectedBooking.NumberOfPeopleOnTour" class="form-control" />
    </div>





    @{
        var currentDeposit = selectedBooking.DepositAmountPaid;
        decimal currentTotal = selectedBooking.TotalPrice;
        var currentCheckOut = selectedBooking.HotelCheckOut;

        switch (selectedBooking.RoomType)
        {
            case "Single":
                newTotal = (hotel.SBPrice * selectedBooking.NumberOfNights) + tour.Cost;
                newTotal = newTotal * (decimal)0.9;
                newDeposit = newTotal / 5;
                newCheckOut = selectedBooking.HotelCheckIn.AddDays(selectedBooking.NumberOfNights);
                break;
            case "Double":
                newTotal = (hotel.DBPrice * selectedBooking.NumberOfNights) + tour.Cost;
                newTotal = newTotal * (decimal)0.8;
                newDeposit = newTotal / 5;
                newCheckOut = selectedBooking.HotelCheckIn.AddDays(selectedBooking.NumberOfNights);
                break;
            case "Family":
                newTotal = (hotel.FamPrice * selectedBooking.NumberOfNights) + tour.Cost;
                newTotal = newTotal * (decimal)0.6;
                newDeposit = newTotal / 5;
                newCheckOut = selectedBooking.HotelCheckIn.AddDays(selectedBooking.NumberOfNights);
                break;
        }


        if(selectedBooking.PaidInfull)
        {
            if (newTotal > currentTotal)
            {
                difference = newTotal - currentTotal;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>You will need to pay £@difference more to complete this booking</p>
                </div>
            }
            else if (newTotal < currentTotal)
            {
                var refund = currentTotal - newTotal;
                difference = refund * -1;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>You will be refunded £@refund</p>
                </div>
            }
        }
        else
        {
            if (newTotal > currentTotal)
            {
                difference = newDeposit - currentDeposit;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>The new deposit for this booking is £@newDeposit</p>
                    <p>You will need to pay £@difference more to complete this booking</p>
                </div>

            }
            else if (newTotal < currentTotal)
            {
                var refund = currentDeposit - newDeposit;
                difference = refund * -1;
                <div>
                    <p>The new total for this booking is £@newTotal</p>
                    <p>The new deposit for this booking is £@newDeposit</p>
                    <p>You will be refunded £@refund</p>
                </div>

            }
        }

        surcharge = newTotal * (decimal)0.05;

        <p>Modifying this booking will also incure a 5% surcharge (£@surcharge)</p>

        if (showHotelOverlap)
        {
            <div class="alert alert-danger" role="alert">
                <p>There are no more spaces for this room on these dates please select new dates.</p>
            </div>
        }

        if (showTourOverlap)
        {
            <div class="alert alert-danger" role="alert">
                <p>There are no more spaces left in this tour for that many people on these dates please select new dates or less people.</p>
            </div>
        }
    }
    <button type="submit" class="btn btn-primary">Save</button>
</EditForm> 


@code {
    [Parameter]
    public string? Id { get; set; }

    private PackageBooking selectedBooking = new PackageBooking();
    private Hotel hotel = new Hotel();
    private Tour tour = new Tour();
    List<string> roomType = new List<string>() { "Single", "Double", "Family" };
    private int currentNights;
    private decimal newTotal, newDeposit;
    private DateTime newCheckOut;
    private int numOfTourOverlap, numOfHotelOverlap;
    private bool showHotelOverlap, showTourOverlap = false;
    private decimal difference;
    private decimal surcharge;

    protected override async Task OnParametersSetAsync()
    {
        if (Id is not null)
        {
            var result = await http.GetFromJsonAsync<PackageBooking>($"api/bookings/package/{Id}");
            if (result is not null)
            {
                selectedBooking = result;
            }
        }

        currentNights = selectedBooking.NumberOfNights;

        hotel = await http.GetFromJsonAsync<Hotel>($"api/hotel/{selectedBooking.HotelId}");
        tour = await http.GetFromJsonAsync<Tour>($"api/tour/{selectedBooking.TourId}");
    }

    async Task HandleSubmit()
    {

        if (selectedBooking.PaidInfull)
        {
            selectedBooking.DepositAmountPaid = newTotal;
            selectedBooking.TotalPrice = newTotal;
        }
        else
        {
            selectedBooking.DepositAmountPaid = newDeposit;
            selectedBooking.TotalPrice = newTotal;
        }
        
        selectedBooking.HotelCheckOut = newCheckOut;

        numOfHotelOverlap = await http.GetFromJsonAsync<int>($"/api/bookings/hotel/overlap?checkIn={selectedBooking.HotelCheckIn}&checkOut={selectedBooking.HotelCheckOut}&hotelId={hotel.Id}&roomType={selectedBooking.RoomType}");

        numOfTourOverlap = await http.GetFromJsonAsync<int>($"/api/bookings/tour/overlap?start={selectedBooking.TourStartDate}&end={selectedBooking.TourEndDate}&tourId={tour.Id}");

        if ((tour.MaxNumberOfGuests < numOfTourOverlap + selectedBooking.NumberOfPeopleOnTour) & (numOfHotelOverlap > 20))
        {
            showHotelOverlap = true;
            showTourOverlap = true;
        }
        else
        {
            await http.PutAsJsonAsync($"api/bookings/package/{Id}?paymentRemainder={difference}&surcharge={surcharge}", selectedBooking);
            NavigationManager.NavigateTo("/mybookings");
        }
    }
}
```

#### BlazorHotelBooking/Client/Pages/EditTourBooking.razor
```c#
@page "/editbooking/tour/{id}"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize]


<PageTitle> Edit Booking </PageTitle>
<h4>Editing booking for @tour.Name on @selectedBooking.CommencementDate.ToString("dd/MM/yyyy")</h4>


<EditForm Model="selectedBooking" OnSubmit="HandleSubmit">
    <div>
        <label for="TourName">Tour</label>
        <InputText id="TourName" @bind-Value="tour.Name" class="form-control" disabled="true" />
    </div>
    <div>
        <label for="NumOfPeople">Number Of Guests</label>
        <InputNumber id="NumOfPeople" @bind-Value="selectedBooking.NumberOfPeople" class="form-control" />
    </div>
   
    @{
        var total = selectedBooking.TotalPrice;
        surcharge = (double)total * 0.05;
    
        <p>Modifying this booking will also incure a 5% surcharge (£@surcharge)</p>

        if (showOverlap)
        {
            <div class="alert alert-danger" role="alert">
                <p>There are no more spaces for this tour for that many people.</p>
            </div>
        }
    }
    <button type="submit" class="btn btn-primary">Save</button>
</EditForm> 


@code {
    [Parameter]
    public string? Id { get; set; }
    private TourBooking selectedBooking = new TourBooking();
    private Tour tour = new Tour();
    private int currentNights;
    private decimal newTotal, newDeposit;
    private DateTime newCheckOut;
    private bool showOverlap = false;
    private int numOfOverlap;
    private int currentPeople;
    private double surcharge;

    protected override async Task OnParametersSetAsync()
    {
        if (Id is not null)
        {
            var result = await http.GetFromJsonAsync<TourBooking>($"api/bookings/tour/{Id}");
            if (result is not null)
            {
                selectedBooking = result;
            }
        }

        currentPeople = selectedBooking.NumberOfPeople;

        tour = await http.GetFromJsonAsync<Tour>($"api/tour/{selectedBooking.TourId}");
    }

    async Task HandleSubmit()
    {

        numOfOverlap = await http.GetFromJsonAsync<int>($"/api/bookings/tour/overlap?start={selectedBooking.CommencementDate}&end={selectedBooking.EndDate}&tourId={tour.Id}");

        if (tour.MaxNumberOfGuests < numOfOverlap + (selectedBooking.NumberOfPeople - currentPeople))
        {
            showOverlap = true;
        }
        else
        {
            await http.PutAsJsonAsync($"api/bookings/tour/{Id}?surcharge={surcharge}", selectedBooking);
            NavigationManager.NavigateTo("/mybookings");
        }
    }  
}
```


#### BlazorHotelBooking/Client/Pages/HotelDetails.razor
```c#
@page "/hotels/{id}"
@using BlazorHotelBooking.Shared;
@using Microsoft.AspNetCore.Authorization;
@using Microsoft.AspNetCore.Http;
@using Syncfusion.Blazor.DropDowns;
@using System.ComponentModel.DataAnnotations;
@using System.Security.Claims
@inject IHttpContextAccessor httpContextAccessor
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize]


<div class="media">
    <div class="media-body">
        <h2 class="mb-0">@selectedHotel.Name</h2>
        <p>@selectedHotel.Description</p>
        <h4 class="price">
            Single Room: £@selectedHotel.SBPrice (per night)
        </h4>
        <h4 class="price">
            Double Room: £@selectedHotel.DBPrice (per night)
        </h4>
        <h4 class="price">
            Family Room: £@selectedHotel.FamPrice (per night)
        </h4>
        <button @onclick="ShowBookingForm" class="btn btn-primary"><i class="oi oi-cart">&nbsp;&nbsp;&nbsp; Book Hotel</i></button>
    </div>
</div>

@if(showBookingForm)
{
    <div class="backgroundPopupBox">
        <div class="popupCreate">
            <EditForm Model=@newBooking OnValidSubmit=BookHotel>
                <DataAnnotationsValidator />
               @*  <ValidationSummary /> *@
               
                <div class="form-group">
                    <label class="control-label">Check-In Date</label>
                    <InputDate @bind-Value="newBooking.CheckIn" min="@min" max="@max" Placeholder="Enter Date" />
                    <ValidationMessage For="@(() => newBooking.CheckIn)" />
                </div>
                <div class="form-group mt-3 col-sm-10">
                    <label class="control-label">Room Type</label>
                    <InputSelect id="roomType" @bind-Value="newBooking.RoomType" placeholder="Room Type">
                        <option value="">---</option>
                        @foreach (var room in roomType)
                        {
                            <option value="@room">@room</option>
                        }
                    </InputSelect>
                    <ValidationMessage For="@(() => newBooking.RoomType)" />
                </div>
                <div class="form-group mt-3">
                    <label class="control-label">Number of nights</label>
                    <InputNumber id="NumOfNights" @bind-Value="newBooking.NumberOfNights" class="form-control" />
                    <ValidationMessage For="@(() => newBooking.NumberOfNights)" />
                </div>
                
                @{
                    switch (newBooking.RoomType)
                    {
                        case "Single":
                            newBooking.TotalPrice = newBooking.NumberOfNights * selectedHotel.SBPrice;
                            break;
                        case "Double":
                            newBooking.TotalPrice = newBooking.NumberOfNights * selectedHotel.DBPrice;
                            break;
                        case "Family":
                            newBooking.TotalPrice = newBooking.NumberOfNights * selectedHotel.FamPrice;
                            break;

                    }
                    newBooking.DepositAmountPaid = newBooking.TotalPrice / 5 ;
                    newBooking.CheckOut = newBooking.CheckIn.AddDays(newBooking.NumberOfNights);
                  
                }

                <div>Check In Date: @newBooking.CheckIn.ToString("dd/MM/yyyy")</div>
                <div>Check Out Date: @newBooking.CheckOut.ToString("dd/MM/yyyy")</div>


                <div>Total Price is £@newBooking.TotalPrice</div>
                <div>Total to pay today (20%) £@newBooking.DepositAmountPaid</div>


                @if(showOverlap)
                {
                    <div class="alert alert-danger" role="alert">
                        <p>There are no more spaces for this room on these dates please select new dates.</p>
                    </div>
                }

                <button type="button" class="btn btn-success" @onclick="BookHotel">Book</button>

            </EditForm>

            <div class="form-group">
                <button class="btn btn-danger" @onclick="ClosePopup">Cancel</button>
            </div>
        </div>
    </div>

}


@code {
    private Hotel selectedHotel = new Hotel();
    private HotelBooking newBooking = new HotelBooking();
    private bool showBookingForm = false;
    private string min;
    private string max;
    private string userId;
    private int numOfOverlap;
    private bool showOverlap = false;

    [CascadingParameter]
    private Task<AuthenticationState>? authenticationState { get; set; }

    [Parameter]
    public string? Id { get; set; }

    List<Hotel> hotels = new List<Hotel>();
    List<string> roomType = new List<string>() { "Single", "Double", "Family" };

    protected override async Task OnInitializedAsync()
    {
        min = DateOnly.FromDateTime(DateTime.Now.Date.AddMonths(2)).ToString("yyyy-MM-dd");
        max = DateOnly.FromDateTime(DateTime.Now.Date.AddYears(5)).ToString("yyyy-MM-dd");

        var authState = await authenticationState;
        var user = authState?.User;
        userId = user?.FindFirst(c => c.Type == ClaimTypes.NameIdentifier)?.Value;

        var result = await http.GetFromJsonAsync<List<Hotel>>("/api/hotel");
        if (result != null)
        {
            hotels = result;
        }

        selectedHotel = hotels.FirstOrDefault(h => h.Id == Int32.Parse(Id));
    }

    private void ShowBookingForm()
    {
        showBookingForm = true;
    }

    async Task BookHotel()
    {

        //code to check if the newbooking overlaps with existing bookings
        numOfOverlap = await http.GetFromJsonAsync<int>($"/api/bookings/hotel/overlap?checkIn={newBooking.CheckIn}&checkOut={newBooking.CheckOut}&hotelId={selectedHotel.Id}&roomType={newBooking.RoomType}");

        if (numOfOverlap > 20)
        {
            showOverlap = true;
        } 
        else 
        {
            newBooking.HotelId = selectedHotel.Id;
            newBooking.UserId = userId;
            newBooking.PaymentDueDate = newBooking.CheckIn.AddDays(-28);

            await http.PostAsJsonAsync("/api/bookings/hotel/book", newBooking);

            ClosePopup();
            NavigationManager.NavigateTo("/mybookings");
        }
    }

    private void ClosePopup()
    {
        showBookingForm = false;
    }
 }
```


#### BlazorHotelBooking/Client/Pages/HotelEdit.razor
```c#
@page "/hoteledit"
@page "/hoteledit/{id:int}"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@inject HttpClient http
@inject NavigationManager NavigationManager
@attribute [Authorize(Roles="Admin")]

@if (Id is null)
{
    <PageTitle>Add New Hotel</PageTitle>
    <h3>Add New Hotel</h3>
}
else
{
    <PageTitle> Edit @selectedHotel.Name </PageTitle>
    <h3>@selectedHotel.Name</h3>
}

<EditForm Model="selectedHotel" OnSubmit="HandleSubmit">

    <div>
        <label for="Id">Id</label>
        <InputNumber id="Id" @bind-Value="selectedHotel.Id" class="form-control" disabled="true" />
    </div>
    <div>
        <label for="Name">Name</label>
        <InputText id="Name" @bind-Value="selectedHotel.Name" class="form-control" />
    </div>
    <div>
        <label for="SBPrice">Single Room Price</label>
        <InputNumber id="SBPrice" @bind-Value="selectedHotel.SBPrice" class="form-control" />
    </div>
    <div>
        <label for="DBPrice">Doule Room Price</label>
        <InputNumber id="DBPrice" @bind-Value="selectedHotel.DBPrice" class="form-control" />
    </div>
    <div>
        <label for="FamPrice">Family Room Price</label>
        <InputNumber id="FamPrice" @bind-Value="selectedHotel.FamPrice" class="form-control" />
    </div>
    <div>
        <label for="Description">Description</label>
        <InputText id="Description" @bind-Value="selectedHotel.Description" class="form-control" />
    </div>
    <button type="submit" class="btn btn-primary">Save</button>
    @if(Id is not null)
    {
        <button type="button" class="btn btn-danger" @onclick="DeleteHotel">Delete</button>
    }
</EditForm>


@code {
    [Parameter]
    public int? Id { get; set; }

    private Hotel selectedHotel = new Hotel { Name = "New Hotel" };

    protected override async Task OnParametersSetAsync()
    {
        if (Id is not null)
        {
            var result = await http.GetFromJsonAsync<Hotel>($"api/hotel/{Id}");
            if (result is not null)
            {
                selectedHotel = result;
            }
        }
    }

    async Task DeleteHotel()
    {
        await http.DeleteAsync($"api/Admin/hotel/{Id}");
        NavigationManager.NavigateTo("/admin");
    }

    async Task HandleSubmit()
    {
        if(Id is null)
        {
            await http.PostAsJsonAsync("/api/Admin/hotel", selectedHotel);
        }
        else
        {
            await http.PutAsJsonAsync($"api/Admin/hotel/{Id}", selectedHotel);
        }
        NavigationManager.NavigateTo("/admin");
    }
}
```

#### BlazorHotelBooking/Client/Pages/Hotels.razor
```c#
@page "/hotels"
@using BlazorHotelBooking.Shared
@using Microsoft.AspNetCore.Authorization
@inject HttpClient http
@attribute [Authorize]

<h3>Hotels </h3>

<input @oninput="Search" placeholder="Search..." class="p-3" />

<br />
<br />
<EditForm Model="checkin" OnSubmit="DateSearch">
    <div class="form-group">
        <label class="control-label">Check-In Date</label>
        <InputDate @bind-Value="checkin" min="@min" max="@max" Placeholder="Enter Date" />
        <label class="control-label">Number Of Nights</label>
        <InputNumber @bind-Value="numofnights" Placeholder="Number of Nights" />
    </div>
    <button type="button" class="btn btn-success" @onclick="DateSearch">Search</button>
</EditForm>


@if (hotels.Count <= 0)
{
    <span> Loading Hotels...</span>
}
else
{
    @foreach (var hotel in hotels)
    {
        <li class="media my-3">
            <div class="media-body">
                <a href="/hotels/@hotel.Id">
                    <h4 class="mb-0">@hotel.Name</h4>
                </a>
                <p>@hotel.Description</p>
                <h6 class="price">
                    Single bed: £@hotel.SBPrice
                </h6>
                <h6 class="price">
                    Double bed: £@hotel.DBPrice
                </h6>
                <h6 class="price">
                    Family bed: £@hotel.FamPrice
                </h6>
            </div>

        </li>
    }
   
}

@code {
    List<Hotel> hotels = new List<Hotel>();
    DateTime checkin = DateTime.Now.Date.AddMonths(2);
    DateTime checkout;
    private int numofnights;
    private string min;
    private string max;
    private int numSingleOver, numDoubleOver, numFamilyOver;


    protected override async Task OnInitializedAsync()
    {

        min = DateOnly.FromDateTime(DateTime.Now.Date.AddMonths(2)).ToString("yyyy-MM-dd");
        max = DateOnly.FromDateTime(DateTime.Now.Date.AddYears(5)).ToString("yyyy-MM-dd");

        var result = await http.GetFromJsonAsync<List<Hotel>>("/api/hotel");
        if (result != null)
        {
            hotels = result;
        }
    }

    private async void Search(ChangeEventArgs args)
    {
        var searchTerm = (string)args.Value;

        var result = await http.GetFromJsonAsync<List<Hotel>>("/api/hotel");
        // make it case insensitive to search hotels

        hotels = result.Where(x => x.Name.IndexOf(searchTerm, StringComparison.OrdinalIgnoreCase) >= 0)
               .OrderByDescending(x => x.Id)
               .ToList();

        StateHasChanged();
    }


    private async void DateSearch()
    {
        var result = await http.GetFromJsonAsync<List<Hotel>>("/api/hotel");
        var tempList = new List<Hotel>();
        checkout = checkin.AddDays(numofnights);

        if (result != null)
        {
            foreach (var hotel in result)
            {
                numSingleOver = await http.GetFromJsonAsync<int>($"/api/bookings/hotel/overlap?checkIn={checkin}&checkOut={checkout}&hotelId={hotel.Id}&roomType=Single");
                numDoubleOver = await http.GetFromJsonAsync<int>($"/api/bookings/hotel/overlap?checkIn={checkin}&checkOut={checkout}&hotelId={hotel.Id}&roomType=Double");
                numFamilyOver = await http.GetFromJsonAsync<int>($"/api/bookings/hotel/overlap?checkIn={checkin}&checkOut={checkout}&hotelId={hotel.Id}&roomType=Family");

                if (!(numSingleOver > 20 && numDoubleOver > 20 && numFamilyOver > 20))
                {
                    tempList.Add(hotel);
                }
            }

            hotels = tempList;
        }
        StateHasChanged();
    }

}
```

#### BlazorHotelBooking/Client/Pages/Index.razor
```c#

```
