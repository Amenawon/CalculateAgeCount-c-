# CalculateAgeCount-c#


    class Program
    {
        static void Main(string[] args)
        {
            WebRequest request = WebRequest.Create("https://coderbyte.com/api/challenges/json/age-counting");
            //WebRequest request = WebRequest.Create("https://coderbyte.com/api/challenges/json/json-cleaning");
            WebResponse response = request.GetResponse();
            using (Stream dataStream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(dataStream);
                string responseFromServer = reader.ReadToEnd();
                //Console.WriteLine(responseFromServer);

                var count = GetAges(responseFromServer);
                var filtered = RemoveInvalidValues(responseFromServer);
                Console.WriteLine(count);
                //Console.WriteLine(filtered);
            }

            response.Close();
        }

        private static string RemoveInvalidValues(string text)
        {
            string emptyValues = "(\"\"|\"[-]\"|\"N\\\\/A\")";
            string placeholder = "#empty#";
            string emptyKeyValues = "(\"[a-zA-Z]+\":#empty#,|,\"[a-zA-Z]+\":#empty#)";
            string emptyArrayValues = "#empty#,|,#empty#";
            string result = Regex.Replace(text, emptyValues, placeholder);
            result = Regex.Replace(result, emptyKeyValues, "");
            result = Regex.Replace(result, emptyArrayValues, "");

            return result;
        }

        private static int GetAges(string text)
        {
            string matchEmailPattern = @"(age=)([5-9][0-9]|[1-9][0-9][0-9])";
            Regex rx = new Regex(matchEmailPattern, RegexOptions.Compiled | RegexOptions.IgnoreCase);
            MatchCollection matches = rx.Matches(text);
            
            return matches.Count;
        }
}
