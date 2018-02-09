## Description
Copies a string to the user's clipboard. This is the same behavior as if the user has copied any string themselves with right click + copy or 
`ctrl + c`.

## Code
```java
public static void setUserClipboard(String string)
{
  Toolkit.getDefaultToolkit()
            .getSystemClipboard()
            .setContents(
            new StringSelection(string),
            null);
}
```