# Message authentication code

Due to your insane hacking skills, the bank has decided to hire you to solve all of their problems.

Your task is to implement a message authentication code (MAC) for the bank, which can be used to verify messages. To help you do this, there is a library function available: Hash.hash(message). This function returns a hash for the message you put into it.

Your MAC should be built using a key, and should be resistant to replay attacks. This means that every time a MAC is generated for a message, the MAC should be different.

### Task 1

You should implement the function addMac(message,key, messageLength). This function should return the message with a MAC added.

### Task 2

The second function to implement is checkMac(message,key, messageLength). This function takes a message with your generated MAC and checks if the MAC is correct. When a message is replayed against your system, this function should label it as not correct and therefore return false.

(Hint: You can use a counter to make MACs unique for the same input.)

### Challenge task for 100/100

With the other two tasks, you can receive a score of 90/100.

If you want 100/100, you will have to implement the function forgeMac(message). To do this, you have access to the library function that checks your answer: MacLib.checkMac(message, mac). This function is vulnerable to a timing attack. The length of the MAC is 16 Hexadecimal characters (make sure to pad your guess to this length).

Note that due to our implementation of the checkMac function, running this attack might take some time.

__________________________________________________________________________________________________________________________________

__Template:__
```java
class Solution {

  // Add a message autentication code to the message using a specific key.
  public static String addMac(String message, String key, int messagelength) {
  // TODO
  }

  // Verify a message autentication code to the message given a specific key.
  public static boolean checkMac(String message, String key, int messagelength) {
  // TODO
  }

  // Forge a MAC based on the timing differences between messages.
  public static String forgeMac(String message) {
  // TODO
  }
}

```
