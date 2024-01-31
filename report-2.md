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
