<?php

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
  sleep(30);
});
