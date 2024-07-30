# 🌟 Fast Response with Post-Response Operations in PHP

If you find this useful, please give it a star! ⭐

This repository demonstrates a method for handling long operations after sending a response to the user in PHP, allowing for faster user interactions without closing the HTTP connection.

## Usage

The `runAfterResponse` function allows you to execute a callback function after sending the main response to the client. This is particularly useful for tasks that can take time, such as sending emails, without delaying the response to the user.

```php
function runAfterResponse(callable $callback)
{
    register_shutdown_function(function () use ($callback) {
        $size = ob_get_length();
        @header("Content-Encoding: none");
        @header("Content-Length: {$size}");
        @ob_end_flush();
        @ob_flush();
        @flush();

        $callback();
    });
}

echo 'hi';

runAfterResponse(function(){
  //send email
  //send push notification
  //do some heavy sql commands
  sleep(30); // Simulate a long operation, e.g., sending an email
});
```

## Features
- **Fast Response**: Sends a response to the user immediately, while the long operation continues in the background.
- **Context Preservation**: Keeps the context intact, allowing the use of `$this` inside a class.
- **No Extra Setup**: No additional configuration or dependencies are required.
- **HTTP/2 Compatibility**: Maintains the connection with the user, making it compatible with HTTP/2.

## How It Works
The method leverages PHP's `register_shutdown_function` to perform operations after the output buffer has been flushed and the response has been sent to the user. This approach is ideal for executing tasks like sending emails or processing data without making the user wait.

## Contributing
Feel free to fork this repository and submit pull requests. Contributions are always welcome!

## License
This project is licensed under the MIT License.

---

