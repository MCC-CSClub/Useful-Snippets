## Description
This snippet is awesome if you have data that you'd like to upload to somewhere (`hastebin.com`). This website is very similar to `pastebin.com`, but with less distractions and a non-limiting API. This means you can upload often, without the need to register an API key 

## Code
```csharp
/**
 * Returns a hastebin link whose contents are the input string.
 *
 * @param input Input string
 * @return String hastebin link if sucessful, "" if not.
 */
public static string UploadStrToHastebin(String input)
{
    using (var client = new WebClient())
    {
        client.Headers[HttpRequestHeader.ContentType] = "text/plain";

        var response = client.UploadString("https://hastebin.com/documents", input);
        JObject obj = JObject.Parse(response);

        if (!obj.HasValues)
        {
            return "";
        }

        string key = (string)obj["key"];
        string hasteUrl = "https://hastebin.com/" + key + ".txt";

        return hasteUrl;
    }
}
```