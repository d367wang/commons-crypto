#### src/main/java/org/apache/commons/crypto/stream/CtrCryptoInputStream.java : 

```
public void seek(long position) throws IOException {
   Utils.checkArgument(position >= 0, "Cannot seek to negative offset.");
   checkStream();
   /*
    * If data of target pos in the underlying stream has already been read
    * and decrypted in outBuffer, we just need to re-position outBuffer.
    */
   if (position >= getStreamPosition() && position <= getStreamOffset()) {
       int forward = (int) (position - getStreamPosition());
       if (forward > 0) {
           outBuffer.position(outBuffer.position() + forward);
       }
   } else {
       input.seek(position);
       resetStreamOffset(position);
   }
}

```

`position` can be arbitrary long integer, `(int) (position - getStreamPosition())` is not safe