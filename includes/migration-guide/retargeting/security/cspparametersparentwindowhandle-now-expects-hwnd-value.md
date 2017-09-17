### CspParameters.ParentWindowHandle now expects HWND value

|   |   |
|---|---|
|Details|The <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle> value, introduced in .NET Framework 2.0, allows an application to register a parent window handle value such that any UI required to access the key (such as a PIN prompt or consent dialog) opens as a modal child to the specified window.Starting with apps that target the .NET Framework 4.7, a Windows Forms application can set the <xref:System.Security.Cryptography.CspParameters.ParentWindowHandle> property with code like the following:<pre><code>cspParameters.ParentWindowHandle = form.Handle;</code></pre>In previous versions of the .NET Framework, the value was expected to be an <xref:System.IntPtr?displayProperty=name> representing a location in memory where the [HWND](https://msdn.microsoft.com/library/windows/desktop/aa383751(v=vs.85).aspx#HWND) value resided. Setting the property to form.Handle on Windows 7 and earlier versions had no effect, but on Windows 8 and later versions, it results in a &quot;<xref:System.Security.Cryptography.CryptographicException?displayProperty=name>: The parameter is incorrect.&quot;|
|Suggestion|Applications targeting .NET 4.7 or higher wishing to register a parent window relationship are encouraged to use the simplified form:<pre><code>cspParameters.ParentWindowHandle = form.Handle;</code></pre>Users who had identified that the correct value to pass was the address of a memory location which held the value <code>form.Handle</code> can opt out of the behavior change by setting the AppContext switch <code>Switch.System.Security.Cryptography.DoNotAddrOfCspParentWindowHandle</code> to <code>true</code>.<ul><li>By programmatically setting compat switches on the AppContext, as explained in this [blog post](http://blogs.msdn.com/b/dotnet/archive/2015/04/29/net-announcements-at-build-2015.aspx#dotnet46).</li><li>By adding the following line to the <code>&lt;runtime&gt;</code> section of the app.config file:</li></ul><pre><code>&lt;runtime&gt;<br />&lt;AppContextSwitchOverrides value=&quot;Switch.System.Security.Cryptography.DoNotAddrOfCspParentWindowHandle=true&quot;/&gt;<br />&lt;/runtime&gt;</code></pre>Conversely, users who wish to opt in to the new behavior on the .NET Framework 4.7 runtime when the application loads under older .NET Framework versions can set the AppContext switch to <code>false</code>.|
|Scope|Minor|
|Version|4.7|
|Type|Retargeting|
|Affected APIs|<ul><li><xref:System.Security.Cryptography.CspParameters.ParentWindowHandle?displayProperty=fullName></li></ul>|
