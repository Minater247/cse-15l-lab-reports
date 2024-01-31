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

├── `url.getPath().equals("/")` is false, as the path is `/add-message`
<br>
├── `url.getPath().equals("/add-message")` is true
<br>
│   ├── `url.getQuery().split("&")` separates the query into message and user, storing in the array `parameters`. The resulting array here is `["s=Now talking in channel main.", "user=System"]`
<br>
│   ├── `try/catch` block for error handling
<br>
│   │   ├── `String user` and `String message` are initialized
<br>
│   │   ├── The portion of the query before the first '&' (`parameters[0]`) is split at the '=' into another array, `first`, containing `["s", "Now talking in channel main."]`
<br>
│   │   ├── The portion of the query after the first '&' and before the second '&' (`parameters[1]`) is split at the '=' into another array, `second`, containing `["user", "System"]`
<br>
│   │   ├── `first[0].equals("s")` is true and `second[0].equals("user")` are true, meaning the message comes before the user in this input
<br>
│   │   │   ├── The `state` variable, containing the message state, is incremented in the format `user: message` by taking the second item of `first` (`first[1]` as the message and the second item of `second` (`second[1]`) as the username. The final added string is `System: Now talking in channel main.\n`.
<br>


<img width="845" alt="Screenshot 2024-01-25 133832" src="https://github.com/Minater247/cse-15l-lab-reports/assets/45747191/e4f5a855-6d8b-4363-9810-84558c12ef87">

<hr>

`handleRequest("https://0-0-0-0-4000-613464ujop807gmt5kq4uklcn0.us.edusercontent.com/add-message?s=Welcome @minater247!&user=mrbean22")` passes a URI containing the URL to the function.

├── `url.getPath().equals("/")` is false, as the path is `/add-message`
<br>
├── `url.getPath().equals("/add-message")` is true
<br>
│   ├── `url.getQuery().split("&")` separates the query into message and user, storing in the array `parameters`. The resulting array here is `["s=Welcome @minater247!", "user=mrbean22"]`
<br>
│   ├── `try/catch` block for error handling
<br>
│   │   ├── `String user` and `String message` are initialized
<br>
│   │   ├── The portion of the query before the first '&' (`parameters[0]`) is split at the '=' into another array, `first`, containing `["s", "Welcome @minater247!"]`
<br>
│   │   ├── The portion of the query after the first '&' and before the second '&' (`parameters[1]`) is split at the '=' into another array, `second`, containing `["user", "mrbean22"]`
<br>
│   │   ├── `first[0].equals("s")` is true and `second[0].equals("user")` are true, meaning the message comes before the user in this input
<br>
│   │   │   ├── The `state` variable, containing the message state, is incremented in the format `user: message` by taking the second item of `first` (`first[1]` as the message and the second item of `second` (`second[1]`) as the username. The final added string is `mrbean22: Welcome @minater247!\n`
<br>
