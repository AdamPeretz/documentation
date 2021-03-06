.. code-block:: bash

  curl -s --user 'api:YOUR_API_KEY' \
      https://se.api.mailgun.net/v3/domains/YOUR_DOMAIN_NAME/messages/STORAGE_URL \
      -F to='bob@example.com'

.. code-block:: java

 import com.mashape.unirest.http.HttpResponse;
 import com.mashape.unirest.http.JsonNode;
 import com.mashape.unirest.http.Unirest;
 import com.mashape.unirest.http.exceptions.UnirestException;

 public class MGSample {

     // ...

     public static JsonNode resendSimpleMessge() throws UnirestException {

         HttpResponse<JsonNode> request = Unirest.post("https://se.api.mailgun.net/v3/domains/" + YOUR_DOMAIN_NAME + "/messages/{storage_url}")
             .basicAuth("api", API_KEY)
             .field("to", "user@samples.mailgun.org")
             .asJson();

         return request.getBody();
     }
 }

.. code-block:: php

  # Currently, the PHP SDK does not support Resend Messages endpoint.
  # Consider using the following php curl function.
  function resend_message() {
    $ch = curl_init();

    curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
    curl_setopt($ch, CURLOPT_USERPWD, 'api:PRIVATE_API_KEY');
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);

    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'POST');
    curl_setopt($ch, CURLOPT_URL, MESSAGE_STORAGE_URL);
    curl_setopt($ch, CURLOPT_POSTFIELDS, array(
        'to'=> 'bob@example.com'
        )
    );

    $result = curl_exec($ch);
    curl_close($ch);

    return $result;
  }
.. code-block:: py

 def resend_simple_message():
     return requests.post(
         "https://se.api.mailgun.net/v3/YOUR_DOMAIN_NAME/messages/STORAGE_URL",
         auth=("api", "YOUR_API_KEY"),
         data={"to": ["bar@example.com", "YOU@YOUR_DOMAIN_NAME"] })

.. code-block:: rb

    def resend_simple_message
        RestClient.post "https://api:YOUR_API_KEY"\
        "@se.api.mailgun.net/v3/domains/YOUR_DOMAIN_NAME/messages/STORAGE_URL",
        :to => "bar@example.com, YOU@YOUR_DOMAIN_NAME"
    end

.. code-block:: csharp

 using System;
 using System.IO;
 using RestSharp;
 using RestSharp.Authenticators;

 public class SendSimpleMessageChunk
 {

     public static void Main (string[] args)
     {
         Console.WriteLine (ResendSimpleMessage ().Content.ToString ());
     }

     public static IRestResponse ResendSimpleMessage ()
     {
         RestClient client = new RestClient ();
         client.BaseUrl = new Uri ("https://se.api.mailgun.net/v3");
         client.Authenticator =
             new HttpBasicAuthenticator ("api",
                                         "YOUR_API_KEY");
         RestRequest request = new RestRequest ();
         request.AddParameter ("domain", "YOUR_DOMAIN_NAME", ParameterType.UrlSegment);
         request.Resource = "domains/{domain}/messages/STORAGE_URL";
         request.AddParameter ("to", "bar@example.com");
         request.Method = Method.POST;
         return client.Execute (request);
     }

 }

.. code-block:: go

 import (
     "context"
     "github.com/mailgun/mailgun-go/v3"
     "time"
 )

 func ResendMessage(domain, apiKey string) (string, string, error) {
     mg := mailgun.NewMailgun(domain, apiKey)

     ctx, cancel := context.WithTimeout(context.Background(), time.Second*30)
     defer cancel()

     return mg.ReSend(ctx, "STORAGE_URL", "bar@example.com")
 }

.. code-block:: js

 var mailgun = require("mailgun-js");
 var Request = require('mailgun-js/lib/request');
 var api_key = 'YOUR_API_KEY';
 var DOMAIN = 'YOUR_DOMAIN_NAME';
 var mailgun = require('mailgun-js')({apiKey: api_key, domain: DOMAIN});

 var data = {
   "to": 'bar@example.com, alice@example.com',
 };

 var options = {
    host: 'se.api.mailgun.net',
    endpoint: '/v3',
    protocol: 'https:',
    port: 443,
    auth: ['api', api_key].join(':'),
    retry: 1
  };

 var req = new Request(options);

 req.request('POST', `/domains/${DOMAIN}/messages/STORAGE_URL`, data, function (error, body) {
   console.log(body);
 });
