# Website's Subsites Content Downloader
# C#6.0  .NET FRAMEWORK 4.6

### Test and Result

![gif source](https://github.com/shtigran/Website-sSubsitesContentDownloader/blob/master/SaveContent.gif)

----

### Purpose
This program allow You find the inputing website subsites list and then download their all content (HTML code). You must input valid URL of website and program do all for You.

----

### The Program class implementation
```c
class Program
{
      string result = string.Empty;
      // Welcome message
      Console.WriteLine("    __________________________________    ");
      Console.WriteLine("____Welcome Website Content Downloader____\n");
            Console.WriteLine("The program download input Website and it's Subsites all content");
      Console.Write("Please enter the website path: ");
      string path = Console.ReadLine();


      // Checking URL validity

      if (!Uri.IsWellFormedUriString(path, UriKind.RelativeOrAbsolute))
      { Console.WriteLine("Wrong URL path!!!"); }

      else
      {   // Creating strings for operations
        string all = string.Empty;
        string htmlCode = string.Empty;

        using (WebClient client = new WebClient()) // WebClient class inherits IDisposable
        {
          htmlCode = client.DownloadString(path);
          result += htmlCode;
          all = showMatch(htmlCode, @"((http|ftp|https):\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&amp;:/~\+#]*[\w\-\@?^=%&amp;/~\+#])?)");
          Console.WriteLine("------------------------");
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine($"\nThere are the following URLs in {path}: ");
                    Console.WriteLine("(Scanning and Downloading code) \n");
                    Console.ForegroundColor = ConsoleColor.Blue;


                    string[] urls = all.Split(new Char[] { '\n' });
                    string text = string.Empty;                  
                    urls[4] = "http://www.freenet.am/";
                   
                    for (int i = 0; i < urls.Length-1; i++)
                    {
                        Console.WriteLine($"URL {i+1}: {urls[i]}\n");
                        result += $"\n\n{urls[i]} \n {client.DownloadString(urls[i])} "; }
                }
            }
           
            File.WriteAllText($"{Environment.GetFolderPath(Environment.SpecialFolder.Desktop)}\\text.txt", result);
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("\nThe content of all Websites is Downloaded on Your Desktop in text.txt!!! ");
 }
```
### Description of Program class
You must the valid URL. The program verify the URL. The program recieve teh html code of that URL and find out all URLs in that. The finding performed through Regular Expression in showMatch() method. After that the program show all suburls and download their HTML codes. Then all HTML codes of content the program save on Desktop with text.txt name.

----

### The showMatch() method implementation
```C#
private static string showMatch(string text, string expr)
{
      MatchCollection mc = Regex.Matches(text, expr);
      string result = "";
      foreach (Match m in mc)
      {
        result += m.ToString() + "\n";
      }
      return result;
}
```
### The showMatch() methodDescription
Yhe method takes two arguments. Fisrts argument is the text and the second is the Regex expression. And with  regex method find all matches in text and return them. 
