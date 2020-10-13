---
keyword: [upload, ]
---

# Data upload by using BufferedWriter

This topic uses sample code to describe how to upload data by using the BufferedWriter interface.

```
// Initialize the code of MaxCompute and Tunnel.
RecordWriter writer = null;
TableTunnel.UploadSession uploadSession = tunnel.createUploadSession(projectName, tableName);
try {
  int i = 0;
  // Generate a TunnelBufferedWriter instance.
  writer = uploadSession.openBufferedWriter();
  Record product = uploadSession.newRecord();
  for (String item : items) {
    product.setString("name", item);
    product.setBigint("id", i);
    // Call the Write interface to write data.
    writer.write(product);
    i += 1;
  }
} finally {
  if (writer ! = null) {
    // Disable TunnelBufferedWriter.
    writer.close();
  }
}
// Commit the upload session to end the upload.
uploadSession.commit();
```

