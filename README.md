# TCPShutdown-notbrave
 If @error Then                             DllClose($hWs2_32)                             Return SetError(3, 0, 0)                         EndIf                          $aResult[$nAdapter - 1][$iCounter] = _WinAPI_GetString($pbuffer_lpszAddressString)                         If @error And @error &lt;> 10 Then                             DllClose($hWs2_32)                             Return SetError(4, 0, 0)                         EndIf                          $pDnsServerAddress = DllStructGetData($tADAPTER_DNS_SERVER_ADDRESS, "Next")                         If Not ($pDnsServerAddress == $pNull) Then                             $tADAPTER_DNS_SERVER_ADDRESS = DllStructCreate($tagIP_ADAPTER_DNS_SERVER_ADDRESS, $pDnsServerAddress)                         Else                             ExitLoop                         EndIf                     Next                 EndIf                  ;AdapterName                 $aResult[$nAdapter - 1][6] = _WinAPI_GetString(DllStructGetData($tIP_ADAPTER_ADDRESSES, "AdapterName"), False)                 If @error And @error &lt;> 10 Then                     DllClose($hWs2_32)                     Return SetError(4, 0, 0)                 EndIf                  ;Description                 $aResult[$nAdapter - 1][7] = _WinAPI_GetString(DllStructGetData($tIP_ADAPTER_ADDRESSES, "Description"))                 If @error And @error &lt;> 10 Then                     DllClose($hWs2_32)                     Return SetError(4, 0, 0)                 EndIf                  ;MAC                 $sMAC = String(DllStructGetData($tIP_ADAPTER_ADDRESSES, "PhysicalAddress"))                 $aResult[$nAdapter - 1][8] = StringRegExpReplace( _                         StringTrimRight(StringTrimLeft($sMAC, 2), _ ;0x                         StringLen($sMAC) - ((DllStructGetData($tIP_ADAPTER_ADDRESSES, "PhysicalAddressLength") * 2) + 2)), _                         "(.{2})(?!$)", "$1-") ;from 0xG4V4B5B4V2N2000000 to G4-V4-B5-B4-V2-N2                  ;IfType                 Switch DllStructGetData($tIP_ADAPTER_ADDRESSES, "IfType")                     Case $IF_TYPE_ETHERNET_CSMACD                         Switch @OSVersion                             Case "WIN_XP", "WIN_XPe", "WIN_2003"                                 $aResult[$nAdapter - 1][9] = "Ethernet or wireless network interface"                             Case Else                                 $aResult[$nAdapter - 1][9] = "Ethernet network interface"                         EndSwitch                     Case $IF_TYPE_IEEE80211                         $aResult[$nAdapter - 1][9] = "Wireless network interface"                     Case $IF_TYPE_TUNNEL                         $aResult[$nAdapter - 1][9] = "Tunnel type encapsulation network interface"                     Case $IF_TYPE_IEEE1394                         $aResult[$nAdapter - 1][9] = "Firewire network interface"                     Case Else                         $aResult[$nAdapter - 1][9] = "Other type of network interface"                 EndSwitch              EndIf         EndIf          $pbuffer_AdapterAddresses = DllStructGetData($tIP_ADAPTER_ADDRESSES, "Next")         If Not ($pbuffer_AdapterAddresses == $pNull) Then             $tIP_ADAPTER_ADDRESSES = DllStructCreate($tagIP_ADAPTER_ADDRESSES, $pbuffer_AdapterAddresses)          EndIf     Until $pbuffer_AdapterAddresses == $pNull      TCPShutdown()     DllClose($hWs2_32)     Return $aResult EndFunc   ;==>_WinAPI_GetAdaptersAddresses
