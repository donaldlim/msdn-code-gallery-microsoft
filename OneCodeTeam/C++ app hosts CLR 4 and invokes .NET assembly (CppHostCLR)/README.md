# C++ app hosts CLR 4 and invokes .NET assembly (CppHostCLR)
## Requires
- Visual Studio 2010
## License
- MS-LPL
## Technologies
- CLR
## Topics
- Interop
- CLR Hosting
## Updated
- 06/10/2012
## Description

<h1>C&#43;&#43; app hosts CLR 4 and invokes .NET assembly (<span class="SpellE">CppHostCLR</span>)<span style="">
</span></h1>
<h2>Introduction</h2>
<p class="MsoNormal"><span style="">The Common Language Runtime (CLR) allows a level of integration between itself and a host. This C&#43;&#43; code sample demonstrates using the Hosting Interfaces of .NET Framework 4.0 to host a specific version of CLR in the process,
 load a .NET assembly, and invoke the types in the <span style="">assembly. </span>
</span></p>
<p class="MsoNormal"><span style="">The code sample also demonstrates the new In-Process Side-by-Side feature in .NET Framework 4. The .NET Framework 4 runtime, and all future runtimes, are able to run in-process with one another. .NET Framework 4 runtime
 and beyond are also able to run in-process with a<span style="">ny single older runtime. In the</span> words, you will be able to load 4<span style="">.0
</span>and 2.0 in the same process, but you will not be able to load 1.1 and 2.0 in the same process. The code sample hosts .NET runtime 4.0 and 2.0 side by side, and loads a .NET2.0 assembly into the two runtimes.<span style="">
</span></span></p>
<h2>Building the Sample<span style=""> </span></h2>
<p class="MsoNormal"><span style="">The output path of these 3 projects should be same. In this sample, the output path is
<b style="">$(<span class="SpellE">SolutionDir</span>)\$(Configuration)\</b>. </span>
</p>
<h2>Running the Sample</h2>
<p class="MsoListParagraphCxSpFirst" style=""><span style=""><span style="">1.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span>Press <b style="">F5</b> to debug this project<span style="">, and you will see</span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""><img src="59646-image.png" alt="" width="576" height="167" align="middle">
</span><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle"><span style="">The first step demonstrates how to host .NET4.0 and .NET2.0 side by side, and load a .NET2.0 assembly into the two runtimes. It uses .NET Framework 4.0 Hosting interfaces to host a .NET runtime, and use
 the <span class="SpellE"><b style="">ICorRuntimeHost</b></span> interface that was provided in .NET v1.x to load a .NET assembly and invoke its type.
</span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle" style=""><span style=""><span style="">2.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">Press Enter to continue </span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""><img src="59647-image.png" alt="" width="576" height="84" align="middle">
</span><span style=""></span></p>
<p class="MsoListParagraphCxSpLast"><span style="">This step demonstrates using .NET Framework 4.0 Hosting Interfaces to host a .NET runtime, and use the
<span class="SpellE"><b style="">ICLRRuntimeHost</b></span> interface that was provided in .NET v2.0 to load a .NET4.0 assembly and invoke its type. Because
<span class="SpellE"><b style="">ICLRRuntimeHost</b></span> is not compatible with .NET runtime v1.x, the requested runtime must not be v1.x.
</span></p>
<h2>Using the Code<span style=""> </span></h2>
<p class="MsoListParagraphCxSpFirst" style=""><b style=""><span style=""><span style="">1.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span></b><b style=""><span style="">Loading the Common Language Runtime into a Process.
</span></b></p>
<p class="MsoListParagraphCxSpMiddle"><span style="">A host can load the CLR into a process by using one of the following procedures:
</span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""></span></p>
<p class="MsoListParagraphCxSpLast" style="margin-left:72.0pt"><span style=""><span style="">a.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">Call the <span class="SpellE"><b style="">CLRCreateInstance</b></span> function to get either an
<span class="SpellE"><b style="">ICLRMetaHost</b></span> or an <span class="SpellE">
<b style="">ICLRMetaHostPolicy</b></span> interface. </span></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C&#43;&#43;</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">cplusplus</span>

<pre id="codePreview" class="cplusplus">
HRESULT hr;


ICLRMetaHost *pMetaHost = NULL;
wprintf(L&quot;Load and start the .NET runtime %s \n&quot;, pszVersion);


hr = CLRCreateInstance(CLSID_CLRMetaHost, IID_PPV_ARGS(&pMetaHost));
if (FAILED(hr))
{
    wprintf(L&quot;CLRCreateInstance failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst" style="margin-left:72.0pt"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle" style="margin-left:72.0pt"><span style=""></span></p>
<p class="MsoListParagraphCxSpLast" style="margin-left:72.0pt"><span style=""><span style="">b.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">Call the <span class="SpellE"><b style="">ICLRMetaHost</b></span><b style="">::<span class="SpellE">EnumerateInstalledRuntimes</span></b>,
<span class="SpellE"><b style="">ICLRMetaHost</b></span><b style="">::<span class="SpellE">GetRuntime</span></b> or
<span class="SpellE"><b style="">ICLRMetaHostPolicy</b></span><b style="">::<span class="SpellE">GetRequestedRuntime</span></b> method to get a valid
<span class="SpellE"><b style="">ICLRRuntimeInfo</b></span> pointer. </span></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C&#43;&#43;</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">cplusplus</span>

<pre id="codePreview" class="cplusplus">
ICLRRuntimeInfo *pRuntimeInfo = NULL;


// Get the ICLRRuntimeInfo corresponding to a particular CLR version. It 
// supersedes CorBindToRuntimeEx with STARTUP_LOADER_SAFEMODE.
hr = pMetaHost-&gt;GetRuntime(pszVersion, IID_PPV_ARGS(&pRuntimeInfo));
if (FAILED(hr))
{
    wprintf(L&quot;ICLRMetaHost::GetRuntime failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Check if the specified runtime can be loaded into the process. This 
// method will take into account other runtimes that may already be 
// loaded into the process and set pbLoadable to TRUE if this runtime can 
// be loaded in an in-process side-by-side fashion. 
BOOL fLoadable;
hr = pRuntimeInfo-&gt;IsLoadable(&fLoadable);
if (FAILED(hr))
{
    wprintf(L&quot;ICLRRuntimeInfo::IsLoadable failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


if (!fLoadable)
{
    wprintf(L&quot;.NET runtime %s cannot be loaded\n&quot;, pszVersion);
    goto Cleanup;
}

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst" style="margin-left:72.0pt"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle" style="margin-left:72.0pt"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle" style="margin-left:72.0pt"><span style=""><span style="">c.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">Get <span class="SpellE"><b style="">ICorRuntimeHost</b></span> for .NET v1.x, .NET2.0 and .NET 4.0. This interface is provided in .NET v1.x and is compatible with all .NET Frameworks.
</span></p>
<p class="MsoListParagraphCxSpMiddle" style="margin-left:72.0pt"><span style="">Call the
<span class="SpellE"><b style="">ICLRRuntimeInfo</b></span><b style="">::<span class="SpellE">GetInterface</span></b> method. Specify
<span class="SpellE"><b style="">CLSID_CorRuntimeHost</b></span><b style=""> </b>
for the <span class="SpellE">rclsid</span> parameter and <span class="SpellE">
<b style="">IID_ICorRuntimeHost</b></span> for the <span class="SpellE">riid</span> parameter.
</span></p>
<p class="MsoListParagraphCxSpLast"><b style=""><span style=""></span></b></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C&#43;&#43;</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">cplusplus</span>

<pre id="codePreview" class="cplusplus">


// Load the CLR into the current process and return a runtime interface 
// pointer. ICorRuntimeHost and ICLRRuntimeHost are the two CLR hosting  
// interfaces supported by CLR 4.0. Here we demo the ICorRuntimeHost 
// interface that was provided in .NET v1.x, and is compatible with all 
// .NET Frameworks. 
hr = pRuntimeInfo-&gt;GetInterface(CLSID_CorRuntimeHost, 
    IID_PPV_ARGS(&pCorRuntimeHost));
if (FAILED(hr))
{
    wprintf(L&quot;ICLRRuntimeInfo::GetInterface failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Start the CLR.
hr = pCorRuntimeHost-&gt;Start();
if (FAILED(hr))
{
    wprintf(L&quot;CLR failed to start w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst"><b style=""><span style=""></span></b></p>
<p class="MsoListParagraphCxSpMiddle" style="margin-left:72.0pt"><span style=""><span style="">d.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">Get <span class="SpellE"><b style="">ICLRRuntimeHost</b></span><b style="">
</b>for .NET2.0 and .NET 4.0. This interface is provided in .NET2.0 and is not compatible with all .NET v1.x.
</span></p>
<p class="MsoListParagraphCxSpLast" style="margin-left:72.0pt"><span style="">Call the
<span class="SpellE"><b style="">ICLRRuntimeInfo</b></span><b style="">::<span class="SpellE">GetInterface</span></b> method. Specify
<span class="SpellE"><b style="">CLSID_CLRRuntimeHost</b></span><b style=""> </b>
for the <span class="SpellE">rclsid</span> parameter and <span class="SpellE">
<b style="">IID_ICLRRuntimeHost</b></span> for the <span class="SpellE">riid</span> parameter.
</span></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C&#43;&#43;</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">cplusplus</span>

<pre id="codePreview" class="cplusplus">
// Load the CLR into the current process and return a runtime interface 
// pointer. ICorRuntimeHost and ICLRRuntimeHost are the two CLR hosting  
// interfaces supported by CLR 4.0. Here we demo the ICLRRuntimeHost 
// interface that was provided in .NET v2.0 to support CLR 2.0 new 
// features. ICLRRuntimeHost does not support loading the .NET v1.x 
// runtimes.
hr = pRuntimeInfo-&gt;GetInterface(CLSID_CLRRuntimeHost, 
    IID_PPV_ARGS(&pClrRuntimeHost));
if (FAILED(hr))
{
    wprintf(L&quot;ICLRRuntimeInfo::GetInterface failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Start the CLR.
hr = pClrRuntimeHost-&gt;Start();
if (FAILED(hr))
{
    wprintf(L&quot;CLR failed to start w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst" style="margin-left:72.0pt"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle"><b style=""><span style=""></span></b></p>
<p class="MsoListParagraphCxSpMiddle" style=""><b style=""><span style=""><span style="">2.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span></b><b style=""><span style="">Load a .NET assembly and call the static method.</span>
</b></p>
<p class="MsoListParagraphCxSpMiddle" style="margin-left:72.0pt"><span style=""><span style="">a.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">If we get <span class="SpellE"><b style="">ICorRuntimeHost</b></span> interface, we can use following code.</span></p>
<p class="MsoListParagraphCxSpLast"><span style=""></span></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C&#43;&#43;</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">cplusplus</span>

<pre id="codePreview" class="cplusplus">
IUnknownPtr spAppDomainThunk = NULL;
_AppDomainPtr spDefaultAppDomain = NULL;


// The .NET assembly to load.
bstr_t bstrAssemblyName(pszAssemblyName);
_AssemblyPtr spAssembly = NULL;


// The .NET class to instantiate.
bstr_t bstrClassName(pszClassName);
_TypePtr spType = NULL;
variant_t vtObject;
variant_t vtEmpty;


// The static method in the .NET class to invoke.
bstr_t bstrStaticMethodName(L&quot;GetStringLength&quot;);
SAFEARRAY *psaStaticMethodArgs = NULL;
variant_t vtStringArg(L&quot;HelloWorld&quot;);
variant_t vtLengthRet;


// The instance method in the .NET class to invoke.
bstr_t bstrMethodName(L&quot;ToString&quot;);
SAFEARRAY *psaMethodArgs = NULL;
variant_t vtStringRet;


// Get a pointer to the default AppDomain in the CLR.
hr = pCorRuntimeHost-&gt;GetDefaultDomain(&spAppDomainThunk);
if (FAILED(hr))
{
    wprintf(L&quot;ICorRuntimeHost::GetDefaultDomain failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


hr = spAppDomainThunk-&gt;QueryInterface(IID_PPV_ARGS(&spDefaultAppDomain));
if (FAILED(hr))
{
    wprintf(L&quot;Failed to get default AppDomain w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Load the .NET assembly.
wprintf(L&quot;Load the assembly %s\n&quot;, pszAssemblyName);
hr = spDefaultAppDomain-&gt;Load_2(bstrAssemblyName, &spAssembly);
if (FAILED(hr))
{
    wprintf(L&quot;Failed to load the assembly w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Get the Type of CSSimpleObject.
hr = spAssembly-&gt;GetType_2(bstrClassName, &spType);
if (FAILED(hr))
{
    wprintf(L&quot;Failed to get the Type interface w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Call the static method of the class: 
//   public static int GetStringLength(string str);


// Create a safe array to contain the arguments of the method. The safe 
// array must be created with vt = VT_VARIANT because .NET reflection 
// expects an array of Object - VT_VARIANT. There is only one argument, 
// so cElements = 1.
psaStaticMethodArgs = SafeArrayCreateVector(VT_VARIANT, 0, 1);
LONG index = 0;
hr = SafeArrayPutElement(psaStaticMethodArgs, &index, &vtStringArg);
if (FAILED(hr))
{
    wprintf(L&quot;SafeArrayPutElement failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Invoke the &quot;GetStringLength&quot; method from the Type interface.
hr = spType-&gt;InvokeMember_3(bstrStaticMethodName, static_cast&lt;BindingFlags&gt;(
    BindingFlags_InvokeMethod | BindingFlags_Static | BindingFlags_Public), 
    NULL, vtEmpty, psaStaticMethodArgs, &vtLengthRet);
if (FAILED(hr))
{
    wprintf(L&quot;Failed to invoke GetStringLength w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Print the call result of the static method.
wprintf(L&quot;Call %s.%s(\&quot;%s\&quot;) =&gt; %d\n&quot;, 
    static_cast&lt;PCWSTR&gt;(bstrClassName), 
    static_cast&lt;PCWSTR&gt;(bstrStaticMethodName), 
    static_cast&lt;PCWSTR&gt;(vtStringArg.bstrVal), 
    vtLengthRet.lVal);


// Instantiate the class.
hr = spAssembly-&gt;CreateInstance(bstrClassName, &vtObject);
if (FAILED(hr))
{
    wprintf(L&quot;Assembly::CreateInstance failed w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Call the instance method of the class.
//   public string ToString();


// Create a safe array to contain the arguments of the method.
psaMethodArgs = SafeArrayCreateVector(VT_VARIANT, 0, 0);


// Invoke the &quot;ToString&quot; method from the Type interface.
hr = spType-&gt;InvokeMember_3(bstrMethodName, static_cast&lt;BindingFlags&gt;(
    BindingFlags_InvokeMethod | BindingFlags_Instance | BindingFlags_Public),
    NULL, vtObject, psaMethodArgs, &vtStringRet);
if (FAILED(hr))
{
    wprintf(L&quot;Failed to invoke ToString w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Print the call result of the method.
wprintf(L&quot;Call %s.%s() =&gt; %s\n&quot;, 
    static_cast&lt;PCWSTR&gt;(bstrClassName), 
    static_cast&lt;PCWSTR&gt;(bstrMethodName), 
    static_cast&lt;PCWSTR&gt;(vtStringRet.bstrVal));

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst"><span style=""></span></p>
<p class="MsoListParagraphCxSpLast"><span style="">The above C&#43;&#43; code does the same thing as this C# code:
</span></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">csharp</span>

<pre id="codePreview" class="csharp">
Assembly assembly = AppDomain.CurrentDomain.Load(pszAssemblyName);
object length = type.InvokeMember(&quot;GetStringLength&quot;, 
    BindingFlags.InvokeMethod | BindingFlags.Static | 
    BindingFlags.Public, null, null, new object[] { &quot;HelloWorld&quot; });
object obj = assembly.CreateInstance(&quot;CSClassLibrary.CSSimpleObject&quot;);
object str = type.InvokeMember(&quot;ToString&quot;, 
    BindingFlags.InvokeMethod | BindingFlags.Instance | 
    BindingFlags.Public, null, obj, new object[] { });

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst"><span style=""></span></p>
<p class="MsoListParagraphCxSpLast" style="margin-left:72.0pt"><span style=""><span style="">b.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span><span style="">If we get <span class="SpellE"><b style="">ICLRRuntimeHost</b></span> interface, we can use following code.
</span></p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C&#43;&#43;</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">cplusplus</span>

<pre id="codePreview" class="cplusplus">
// The invoked method of ExecuteInDefaultAppDomain must have the 
// following signature: static int pwzMethodName (String pwzArgument)
// where pwzMethodName represents the name of the invoked method, and 
// pwzArgument represents the string value passed as a parameter to that 
// method. If the HRESULT return value of ExecuteInDefaultAppDomain is 
// set to S_OK, pReturnValue is set to the integer value returned by the 
// invoked method. Otherwise, pReturnValue is not set.
hr = pClrRuntimeHost-&gt;ExecuteInDefaultAppDomain(pszAssemblyPath, 
    pszClassName, pszStaticMethodName, pszStringArg, &dwLengthRet);
if (FAILED(hr))
{
    wprintf(L&quot;Failed to call GetStringLength w/hr 0x%08lx\n&quot;, hr);
    goto Cleanup;
}


// Print the call result of the static method.
wprintf(L&quot;Call %s.%s(\&quot;%s\&quot;) =&gt; %d\n&quot;, pszClassName, pszStaticMethodName, 
    pszStringArg, dwLengthRet);

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst" style="margin-left:72.0pt"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""></span></p>
<p class="MsoListParagraphCxSpMiddle"><span style=""></span></p>
<p class="MsoListParagraphCxSpLast" style=""><b style=""><span style=""><span style="">3.<span style="font:7.0pt &quot;Times New Roman&quot;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</span></span></span></b><b style=""><span style="">Clean up the runtime</span>. </b>
</p>
<div class="scriptcode">
<div class="pluginEditHolder" pluginCommand="mceScriptCode">
<div class="title"><span>C#</span></div>
<div class="pluginLinkHolder"><span class="pluginEditHolderLink">Edit</span>|<span class="pluginRemoveHolderLink">Remove</span>
</div>
<span class="hidden">csharp</span>

<pre id="codePreview" class="csharp">
if (pMetaHost)
{
    pMetaHost-&gt;Release();
    pMetaHost = NULL;
}
if (pRuntimeInfo)
{
    pRuntimeInfo-&gt;Release();
    pRuntimeInfo = NULL;
}
if (pCorRuntimeHost)
{
    // Please note that after a call to Stop, the CLR cannot be 
    // reinitialized into the same process. This step is usually not 
    // necessary. You can leave the .NET runtime loaded in your process.
    //wprintf(L&quot;Stop the .NET runtime\n&quot;);
    //pCorRuntimeHost-&gt;Stop();


    pCorRuntimeHost-&gt;Release();
    pCorRuntimeHost = NULL;
}

</pre>
</div>
</div>
<div class="endscriptcode">&nbsp;</div>
<p class="MsoListParagraphCxSpFirst" style="margin-left:72.0pt"></p>
<p class="MsoListParagraphCxSpLast"><b style=""></b></p>
<h2>More Information</h2>
<p class="MsoNormal"><a href="http://msdn.microsoft.com/en-us/library/dd380850.aspx">MSDN: Hosting Overview</a><span style="">
</span></p>
<p class="MsoNormal"><a href="http://msdn.microsoft.com/en-us/magazine/ee819091.aspx">CLR Inside Out: In-Process Side-by-Side</a></p>
<hr>
<div><a href="http://go.microsoft.com/?linkid=9759640" style="margin-top:3px"><img alt="" src="http://bit.ly/onecodelogo">
</a></div>
