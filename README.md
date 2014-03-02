OAEP
====

An implementation of optimal asymmetric encryption padding, to be used in conjunction with RSA.

This is a public domain project so do with it as you will. I make no warranties about this project, so don't come complaining if it breaks/has a bug.

Current Usage
====

    byte[] pad(byte[] message, String params, int length);
    
Returns a padded version of the `message`. `params` is a tokenizable `String` separated by spaces. The first argument should be the name of the hash function used, and the second argument should be the name of the mask function used. An example of a correct `params` string is `SHA-256 MGF1`, which also happens to be the only combination I have supported so far. `length` is simply the desired final length of the message. If you were using RSA-2048, this value would be 256.

Should anything fail, `null` will be returned instead.

NOTE: the `message` length cannot be greater than `length - hLen * 2 - 1` where `hLen` is the size of the output of the hash function in bytes. This will be 32 since only SHA-256 is supported currently and it has an output of 32 bytes. So if you are using RSA-2048 and have a desired `length` of 256 bytes, the biggest message you can send will be `256 - 32 * 2 - 1` or `191` bytes.

    byte[] unpad(byte[] message, String params);
    
Returns an unpadded version of the `message` given the `params` (these parameters should be the same as those used in the `pad` method).

Should anything fail, `null` will be returned instead.

    byte[] MGF1(byte[] seed, int seedOffset, int seedLength, int desiredLength);
    
I've also included an implementation of MGF1 with this code. It takes an input `seed` and an offset (`seedOffset`) and length (`seedLength`) so that only a slice of the seed may be used to generate masks. `desiredLength` is the length of the output, a mask generated from the given seed.