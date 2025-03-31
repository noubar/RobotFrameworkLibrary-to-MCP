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
Run the following Code in a py file in the root folder of library:

```
from src.LibraryName import LibraryName 
a = LibraryName().to_mcp()
a.run(transport='stdio')
```

You can use it in any MCP Client
Here is an example with vscode insiders

1. press ctrl shift p and type MCP:Add Server
1. Add the following lines
```
{
    "mcp": {
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
}
```
Thats it

## Demo

I will be documenting a demo using [MailClientLibrary](https://github.com/noubar/RobotFramework-MailClientLibrary)

```
from src.MailClientLibrary import MailClientLibrary 
a = MailClientLibrary(Username="user", Password="pass", MailServerAddress="127.0.0.1", ImapPorts=[993,143], Pop3Ports=[995,110], SmtpPorts=[465,25]).to_mcp()
a.run(transport='stdio')
print(a)
```
