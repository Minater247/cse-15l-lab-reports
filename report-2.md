# Lab Report 2 - Servers and SSH Keys

## Part 1: A Simple Chat Server

This section implements a simple chat server using the provided file, Server.java, sourced from the [wavelet repository](https://github.com/ucsd-cse15l-f23/wavelet).

```java
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String state = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return state;
        } else if (url.getPath().equals("/add-message")) {
            String[] parameters = url.getQuery().split("&");
            try {
                String user;
                String message;
                String[] first = parameters[0].split("=");
                String[] second = parameters[1].split("=");
                if (first[0].equals("s") && second[0].equals("user")) {
                    state += second[1] + ": " + first[1].replace('+', ' ') + "\n";
                    return state;
                } else if (first[0].equals("user") && second[0].equals("q")) {
                    state += first[1] + ": " + second[1].replace('+', ' ') + "\n";
                    return state;
                }
                return "Invalid input. Please check your query and try again!";
            } catch (Exception e) {
                return "Invalid input. Please check your query and try again!";
            }
        } else {
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
### Screenshots and Tracebacks
<img width="948" alt="Screenshot 2024-01-25 133823" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/1f255aa3-5c3a-4e00-96a1-cf42ce398c64">

<hr>

`handleRequest("https://0-0-0-0-4000-613464ujop807gmt5kq4uklcn0.us.edusercontent.com/add-message?s=Now talking in channel main.&user=System")` passes a URI containing the URL to the function.

- `url.getPath().equals("/")` is false, as the path is `/add-message`
- `url.getPath().equals("/add-message")` is true
- `url.getQuery().split("&")` separates the query into message and user, storing in the array `parameters`. The resulting array here is `["s=Now talking in channel main.", "user=System"]`
- `try/catch` block for error handling, in case of incorrect input causing an `IndexOutOfBoundsException`
- `String user` and `String message` are initialized
- The portion of the query before the first '&' (`parameters[0]`) is split at the '=' into another array, `first`, containing `["s", "Now talking in channel main."]`
- The portion of the query after the first '&' and before the second '&' (`parameters[1]`) is split at the '=' into another array, `second`, containing `["user", "System"]`
- `first[0].equals("s")` is true and `second[0].equals("user")` are true, meaning the message comes before the user in this input
- The `state` variable, containing the message state, is incremented in the format `user: message` by taking the second item of `first` (`first[1]` as the message and the second item of `second` (`second[1]`) as the username. The final added string is `System: Now talking in channel main.\n`.


<img width="845" alt="Screenshot 2024-01-25 133832" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/e4f5a855-6d8b-4363-9810-84558c12ef87">

<hr>

`handleRequest("https://0-0-0-0-4000-613464ujop807gmt5kq4uklcn0.us.edusercontent.com/add-message?s=Welcome @minater247!&user=mrbean22")` passes a URI containing the URL to the function.

- `url.getPath().equals("/")` is false, as the path is `/add-message`
- `url.getPath().equals("/add-message")` is true
- `url.getQuery().split("&")` separates the query into message and user, storing in the array `parameters`. The resulting array here is `["s=Welcome @minater247!", "user=mrbean22"]`
- `try/catch` block for error handling, in case of incorrect input causing an `IndexOutOfBoundsException`
- `String user` and `String message` are initialized
- The portion of the query before the first '&' (`parameters[0]`) is split at the '=' into another array, `first`, containing `["s", "Welcome @minater247!"]`
- The portion of the query after the first '&' and before the second '&' (`parameters[1]`) is split at the '=' into another array, `second`, containing `["user", "mrbean22"]`
- `first[0].equals("s")` is true and `second[0].equals("user")` are true, meaning the message comes before the user in this input
- The `state` variable, containing the message state, is incremented in the format `user: message` by taking the second item of `first` (`first[1]` as the message and the second item of `second` (`second[1]`) as the username. The final added string is `mrbean22: Welcome @minater247!\n`


## Part 2: Secure Shell Keys, SCP and MKDIR
- Path to Local Public Key
<img width="476" alt="Screenshot 2024-01-30 180605" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/e32d1eba-ade8-47e7-a079-77d2769607f2">

- Path to Remote Private Key
<img width="618" alt="Screenshot 2024-01-30 180435" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/0a61ebf0-db77-4356-bc22-2b0d4e3ceaea">

- Login Without Password
<img width="857" alt="Screenshot 2024-01-30 175934" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/bcb9e574-a02a-4787-81ab-95acca9109a3">

## Part 3: What I Learned

Something new I learned this lab was the command `scp`! Although I have programmed frequently using many languages in the past years, I have always worked on my local computer, rarely using ssh or ftp server connections. This command essentially combines the two, allowing direct copying between local/remote and remote/remote locations!
