```shell
USER username
PASS password
QUIT

```

|    Command   |   Description   | Example  |
|-----------|-----------------|---------------|
| USER [username]      | 1st login command |USER Stan  |
|PASS [password]   |   2nd login command   |   PASS SeCrEt |
|QUIT   |   Logs out and saves any changes   |   QUIT |
|LIST   |   Lists all messages   |   LIST |
|RETR [message]   |   Retrieves the whole message   |   RETR 1 |
|DELE [message]   |   Deletes the specified message   |   DELE 2 |
|NOOP   |   The POP3 server does nothing, it merely replies with a positive response.   |   NOOP |
|RSET   |   Undelete the message if any marked for deletion   |   RSET |
|TOP [message] [number]   |   Returns the headers and number of lines from the message   |   TOP 1 10 |

