# Message Modification

With your implementation of the ARP spoof attack, you have entered your favorite bank. 
Luckily for your attack, they do not have any detection methods for the ARP spoof. 
But what they do have, is encryption in their messages.

As it so happens, you conduct a targeted attack on one of the employees of the bank, right at the moment that money is transfered. 
During this attack, you have peeked at their screen and you have seen to which account the money is being transfered. 
Additionally, a secret source has provided you with the format of the packet, and the method of encryption. 
From this you have learned that the bank uses a XOR stream encryption.

The packet format is as follows (all fields are transfered in hexadecimal):

| Opcode: 1 Byte | Sender account number: 8 bytes | Target account number: 8 bytes | Amount: 4 bytes |
|----------------|--------------------------------|--------------------------------|-----------------|

In your attack, you are interested in replacing the target account number with your own account number.
Task

Implement the function modifyMessage(message,targetAccount,yourAccount), in which you modify the message in such a way that it correctly decrypts with your account number in it.

Template:
```java
class Solution {

  // Modify the message to contain your own account in the encrypted message.
  public static String modifyMessage(String message, String targetAccount, String yourAccount) {

  }
}

```
