# RobotFrameworkLibrary-to-MCP
How to turn any robot framework library to a mcp server

# Here is How

1. Clone your robot library
1. open the folder and go to __init__.py, where the Library class with hybrid core or dynamic core is initiated.
    (example)
   ```
   from robotlibcore import DynamicCore
   class LibraryName(DynamicCore):
     def __init__(self):
        ...
   ```
1. Add the following function to the class
```
    def to_mcp(self):
        from mcp.server.fastmcp import FastMCP
        mcp = FastMCP(self.__class__.__name__)
        for kw in self.keywords.values():
            mcp.add_tool(kw)
        return mcp
```

You can use it in any MCP Client
Here is an example with vscode insiders

1. press ctrl shift p and type ```MCP: Open Workspace Folder Configuration```. (Click on create if does not exist)
1. Add the following lines
```
{
        "servers": {
            "LibName": {
                "type": "stdio",
                "command": "uv",
                "args": [
                    "--directory",
                    "C:\\somefolder\\ABS Path to The library Root",
                    "run",
                    "test.py"] # the new py file name where you run the server
            }
        }
}
```
You could run the following Code in a py file in the root folder of library:

```
from src.LibraryName import LibraryName
if __name__ == "__main__":
    LibraryName().to_mcp().run(transport="stdio")
```
Or 

<img width="564" height="246" alt="image" src="https://github.com/user-attachments/assets/274ae0d0-7c93-4838-b35c-4b595188718e" />

Thats it


You can then check it under ctrl shift p with
``` MCP: List Servers ```



## Demo

I will be documenting a demo using [MailClientLibrary](https://github.com/noubar/RobotFramework-MailClientLibrary)

```
from src.MailClientLibrary import MailClientLibrary 
a = MailClientLibrary(Username="user", Password="pass", MailServerAddress="127.0.0.1", ImapPorts=[993,143], Pop3Ports=[995,110], SmtpPorts=[465,25]).to_mcp()
a.run(transport='stdio')
print(a)
```

## CODEX
under project root folder add .cortex/config.toml file with the following input
```
[mcp_servers.mail-client]
command = "uv"
args = [
  "--directory",
  "C:\\Repos\\RobotFramework-MailClientLibrary",
  "run",
  "mail_mcp_server.py",
]
cwd = "C:\\Repos\\RobotFramework-MailClientLibrary"

```

open a new session and ask:
ist all mcp servers you can access
