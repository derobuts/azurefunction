
    /* Author: dbutoyez -- The R.G Mechanics-- 
     * Method recieves a Json object of a unique ticket id adds to the queue and creates a Qr scannable image
     * Send Email asynchronously without blocking**/
    public static class Function1
    {
        private readonly static HttpClient httpClient = new HttpClient();
        public static void updateTicket()
        {

        }
        [FunctionName("EmailANDQRcode")]
        public async static void Run([HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequest req, TraceWriter log)
        {
            try
            {
                string name = req.Query["name"];
                string requestBody = new StreamReader(req.Body).ReadToEnd();
                //convert requestbody from json to clr object
                //create raw qr code
                QRCodeGenerator qrGenerator = new QRCodeGenerator();
                QRCodeData qRCodeData = qrGenerator.CreateQrCode("what", QRCodeGenerator.ECCLevel.L);
                //create byte/raw bitmap qr code
                BitmapByteQRCode qrCodeBmp = new BitmapByteQRCode(qRCodeData);
                byte[] qrCodeImageBmp = qrCodeBmp.GetGraphic(4);
                var h = Convert.ToBase64String(qrCodeImageBmp);
                log.Info("object succesfully converted to base64.")
                
                var client = new SendGridClient(apiKey);
                var message = new SendGridMessage();
                string cidban = "banner";
                message.AddTo("derobuts@outlook.com");
                message.From = new EmailAddress("tbutoyez@gmail.com", "dero");
                message.Subject = "May The Code Be With ";
                message.PlainTextContent = "Pullup";
                message.HtmlContent = "<html><body><p>This Is SPARTA<p/><img src=cid:" + cidban + "/></body></html>";
                message.AddAttachment("DER", h, "image/png", "inline", "banner");
                log.Info("object succesfully added to...");
                var response = await client.SendEmailAsync(message);

                if (response.StatusCode == HttpStatusCode.Accepted)
                {
                    updateTicket();
                }
                else
                /*Log the error to the database and send email to the*/
                {

                }
            }
            catch (WebException e)
            {

                var hs = e.Message.ToString();


            }

        }
        
        
    }
}
