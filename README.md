# Overview

![React-Arc](https://github.com/Michael-DTran/React-Domain/assets/112426094/a783c826-c47c-47f9-ac58-65e8d35e290b)

In this project I will be creating a React App and upload those files to a S3 Bucket to host a static website. Once uploaded I will be making a domain name service under Route 53 (R-53). Once registered I will create a certificate on AWS Certificate Manager (ACM) on a Cloudfront (CF) distribution to enable caching and https. Once those are all set up I will then verify that everthing works. 

# Stage 1: Create React App 
Make a directory folder to store all the React files.
Open Windows Powershell and navigate to your file directory 
Enter the following commands: 
Note: *Make sure you have npm installed*

``` npx create-react-app demo-app ```

![stg1 1](https://github.com/Michael-DTran/React-Domain/assets/112426094/4e4dc74b-a9be-495c-9e43-a118bda9708b)

Once installed enter into the demo-app

``` cd demo-app ```
``` npm start ```

![stg1 3](https://github.com/Michael-DTran/React-Domain/assets/112426094/8e043e92-7105-4389-9188-97b39a1bb7d7)

The react app should start up on a seperate window. This is what I will be deploying on the S3 website. You can close this window afterwards. 

![stg1 4](https://github.com/Michael-DTran/React-Domain/assets/112426094/b0d2e2ad-cbec-4c66-917f-92a0a3b4e0dd)

Bundle all of the files together so they can be deployed in a production environment.

```npm run build``` 
press **y** if prompted.

![stg1 6](https://github.com/Michael-DTran/React-Domain/assets/112426094/91310a47-46b2-4402-bb5c-ca51b5bad7d2)

All the files will be batched into some static files. Enter in ```demo-app --> build``` 
You should see the index.html, logos, manifest, and robots files.

![stg1 7](https://github.com/Michael-DTran/React-Domain/assets/112426094/3ca60703-baba-4a70-8b49-64ed6c4bf3e4)
![stg1 8](https://github.com/Michael-DTran/React-Domain/assets/112426094/8ecf1bc9-5b33-44b7-9b9e-a5d0d59fdd4b)
![stg1 9](https://github.com/Michael-DTran/React-Domain/assets/112426094/4da83375-2ae9-42b7-b6b5-dcdd0c976dbf)


If you delve deeper into ```demo-app --> build --> static```
You should see the css, javascript, and media files. All of these files will be uploaded to S3 to host the website. You can now close Powershell as you do not need it anymore.

![stg1 10](https://github.com/Michael-DTran/React-Domain/assets/112426094/f5cde2b6-13f0-41c2-a016-52a1d383bd0e)

# Stage 2: Create the S3 Bucket
Head to S3: https://s3.console.aws.amazon.com/s3
We will create two different buckets for S3. One will be our website without 'www' (**non-www**) and another with 'www' (**www**).
The **www** website will act as the authority which will host all of our files while the **non-www** is for redirection.

Create the bucket.

![stg2 1](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/34f290ed-5c40-462d-88fd-8c4e51262333)

Name the bucket corresponding to our website url. Select ```US East(N. Virginia) us-east-1``` as our **AWS Region**.
Press on **Create Bucket**.

![stg2 2](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/55a1e01e-3e35-4fdf-aad7-0d8dad9268c9)
![stg2 3](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/e339f12b-4a85-4fa6-a965-4f78e89e0746)

Next, create the second bucket that corresponds with the website name. Select ```US East(N. Virginia) us-east-1``` as our **AWS Region**
Press on **Create Bucket**.

![stg2 4](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/071597e5-6c29-4c3d-9671-16f15b16b805)
![stg2 5](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/e1d417d2-ca92-4eb6-9c05-2c50153eccef)

Once those are both created select the **www** and upload the React files to it. 

![stg2 6](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/787d1432-779e-433a-934d-f9a7de4239fb)

Select **Upload**. Press on **Add files** and add all the files in the ```Demo-App --> build``` directory. 

![stg2 7](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/c6d47422-ced6-467b-b290-f12576e10aec)
![stg2 8](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4b61d5c9-746a-4c4b-aa95-34e52338d70f)
![stg2 9](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/849cb24d-6e33-477e-b568-9b83e72ae539)

Once added select **Add folder** and add the static folder.
![stg2 10](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/f6170303-b67a-4bb5-aa84-8dbc020d8426)
![stg2 11](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/2888a5c1-19a6-4f89-bfc1-cba9ed85a74d)

All the files and folders should be in the table ready for upload.
Press on **Upload**.

![stg2 13](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/80e933ec-c2d7-40e2-a017-a20a41a0c279)

Once the upload succeeds you can press **Close**.

![stg2 14](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/321c898e-8601-44fb-83a3-d5a897ca2632)

Head to the **www** bucket and press on **Permissions**.

![stg2 15](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/e56d64b7-eb3c-450e-8ee5-5320eccbd784)

Head to ```Block public access (bucket settings)``` and press on **Edit**.

![stg2 16](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/881776fd-5bd0-464b-b884-674edc718513)

Uncheck the ```Block all public access``` setting and press on **Save Changes**.

![stg2 17](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/3ffc68ca-5a26-44a8-ba88-4297e52ecfae)

Enter in ```confirm``` and press on **Confirm** 

![stg2 18](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4298409c-d05c-4729-876c-f1d1fb1e398c)

Head to ```Bucket Policy``` and press on **Edit** 

![stg2 19](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/7f63b87a-ea58-41dc-b1a3-807601e30f28)

Paste in this bucket policy and replace ```Bucket-Name``` with the name of the corresponding bucket that has the **www** url. 

Note: *The bucket-policy* file is attached to this repository: https://github.com/Michael-DTran/React-Domain-Proj/blob/6dbb0515bb168820d4e9ca365771f4d0a1dcaed0/bucket-policy

Press on **Save changes** when complete. 
Note: *Ignore the API Error*

![stg2 21](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/8227f8b6-768f-4dad-bb3f-592c2d6ff737)

![stg2 22](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4b2697fa-2e82-4576-84d9-831577462870)

To do a manual check head to the **www** bucket and click on ```index.html```


![stg2 23](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/1a35ab78-261f-418b-b43a-bc5cf1db889c)

![stg2 24](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/ddc429df-0ed1-4594-b3e9-d1a3d6ec5731)

Enter in the ```Object URL```

![stg2 25](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/52b9ce6d-b600-4432-915a-9c52cdcf1f48)

The page should be blank since you don't have any of the javascript or css files added on to it but the React App should still open in this file. 
This test is to show that the bucket is in fact public and that you can access the files.

![stg2 26](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/96527be6-22b7-4ff0-8b20-429b27721161)

Head back to the **www** and press on the ```Properties``` tab. Scroll down until you see ```Static website hosting```. Press on **Edit**.

![stg2 27](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/b763e01b-aad4-43bf-a66a-00a45b5f3910)

![stg2 28](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/3a2a2bb8-62b1-483c-9301-d55f3080b80c)

Change ```Static website hosting```to ```enable``` and ```Hosting type``` to ```Host a static website```. For ```Index document``` type in the ```index.html``` file.

![stg2 29](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/b3e2c992-687b-4544-9bb7-77253f601483)

Press on **Save changes**.

![stg2 30](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/13b6175e-b2e3-4224-94ae-06778c4117d2)

Head to the **non-www** bucket and press on the ```Properties``` tab. Scroll down until you see ```Static website hosting```. Press on **Edit**.

![stg2 31](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/0c2c7b07-462c-4a17-86ad-a6c4e2367a01)

![stg2 32](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/5afb7fd6-d6e5-452c-890b-7b4803a88493)

Change ```Static website hosting```to ```enable``` and ```Hosting type``` to ```Redirect requests for an object```. Under ```Host name``` enter in the bucket with **www**. If you type in a **non-www** it will not lead you to the **www** website. Under ```Protocol - Optional``` change it to ```http``` for now. When we set up the CF distribution we will change it to https later on in this project. 
Click on **Save changes**.

![stg2 33](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/daf638a3-14de-4702-9f6b-7f40a3d092bc)

# Stage 3: Make Route 53 Modifications

We will now register the domain.

Head to Route 53: https://us-east-1.console.aws.amazon.com/route53

Press on **Register Domain**.

![stg3 1](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/6326cf29-9885-4dbf-ba12-a5b66bc3f24a)

Under ```Check availability for a domain``` enter in your desired domain name. Press **Select**.

![stg3 2](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/2b987d1f-77c6-4a7f-bf42-5202c5d305db)

Follow the prompts for pricing and press **Next**.

![stg3 3](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/a4ebcad2-bd8f-4d65-829c-72fb5b349b7f)

Enter in your payment information and contact information. Agree to the ```Terms and conditions```. Press **Submit**.

![stg3 4](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/964cb61a-1d4d-4299-9051-32b403a311df)

Note: *The domain may take from a few minutes to a few days to register. Message AWS if you have issues getting your domain registered in a reasonable amount of time*.
Once your domain is registered the ```status``` should read ```Domain registration successful```.
Click on **Hosted Zone**.

![stg3 5](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/3d591898-1d7c-474e-9165-a8e74cd5d2ca)

Click on the domain name under ```Hosted zone name```.

![stg3 6](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/32b331f0-d393-4050-8525-0d0c1e9a0069)

You should see two records. One with type ```NS``` or ```Name Server``` and one with ```SOA``` or ```State of Authority``` made when we registered the domain. 
We will be creating two more records. Press on **Create Record**.

![stg3 7](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/10546d3b-7e8b-45b5-929b-4d0550bedab8)

Under ```Choose routing policy``` from the selection wizard, select ```Simple routing``` and press **Next**.

![stg3 8](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/2c3bfb98-925d-4aae-8988-f55407bfd365)

Press on **Define simple record**.

![stg3 9](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/800040af-fcc7-4417-bea6-d83da91b95a0)

Enter in **www** for the ```Record name```. ```Record type``` should be ```A - Routes traffic to an IPV4 address and some AWS resources```.
```Value/Route traffic to``` ```Alias to S3 website endpoint``` in the ```US East (N.Virginia) [us-east-1]``` region and set to ```s3-website-us-east-1.amazonaws.com```.
Set ```Evaluate target health``` to **No**. Press on **Define simple record**.

![stg3 10](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/719ec078-0726-46b5-abf2-da2d815ae6e0)

Press on **Define simple record** again and repeat this step for the **non-www**.

![stg3 11](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/58af5918-b56b-47f1-898d-498eca662ad9)

All the steps are the same as before except leave the ```Record name``` blank. 

![stg3 10x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/5c255add-7051-4bfb-8af6-ec565945811e)

Once both records are created press on **Create records**.

![stg3 12](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/7440ee5f-7db6-417e-9269-9d5668819901)

You should have four records in total with two new ```A``` records for both the **www** and **non-www**.

![stg3 13](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/856dbe46-5df3-4aa5-8713-801ca976f4f7)

Set up a new tab and enter in the website. You should be able to enter but have a notification on the top saying ```Not secure```. The website isn't secure by Cloudfront for https yet. We will add this in the next following steps.

![stg3 15](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/d0258180-f6ae-454e-8964-61dd15ecf4bb)

# Stage 4: Set Up Certificates with AWS Certificate Manager
We need to set up certficates to set up our https. 
Head to ACM: https://us-east-1.console.aws.amazon.com/acm
Press on **Request a certificate**.

![stg4 1](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/eb2fdea7-3a9d-4a60-8405-f3d7020a0a54)

Under ```Certficate type``` press on ```Request a public certificate```. Press on **Next**.

![stg4 2](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/5fcfd5c8-447f-4868-87e4-abd2b52122bd)

Under ```Fully qualified domain name``` enter in your **www** website url. Press on **Add another name on this certificate**.

![stg4 3](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/363245d1-0a11-4663-8acd-0daabbaf2d52)

Enter in the **non-www** website url. 

![stg4 4](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/f26f97cf-ad98-4d96-8ae8-4c3b9cd1c2f4)

Scroll down to ```DNS validation - recommended``` and select it. Scroll down and press *Request*.

![stg4 5](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/ef40991f-be29-4a22-ae52-c057be9f309a)

When the ACM certifcate is complete you should see both entries with a status of ```Success```.

![stg4 9](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/649d5e7b-f219-4166-8040-57112c532865)

Head to Route53: https://us-east-1.console.aws.amazon.com/route53
Under ```Hosted zones``` and under ```Records``` you should see two ```CNAME``` records. These values should match with the certificates back on ACM under ```CNAME value```.

![stg4 8](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/1ff9a8c7-5109-468f-9884-67960cb4e44e)

If you enter in the website url and press ```F12``` on your keyboard you should open the console. Head to ```Network --> Headers``` click on the **www** url under ```Name```. 
On ```Server``` the distribution should read ```AmazonS3```. We will come back to this console and observe the changes after we have set up the Cloudfront distribution.

![stg4 12x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/440e07f7-96f0-4a31-8d94-8d3bb7463924)

# Stage 5: Set up Cloudfront for HTTPS
Cloudfront will cache your data to edge locations so users from around the world will always get optimal performance when logging into our website. 
Head to Cloudfront: https://us-east-1.console.aws.amazon.com/cloudfront
We will make our **www** Cloudfront distribution first. Click on **Create a Cloudfront distribution**.

![stg5 1](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/944a3f2a-117c-4509-ad58-2a12f65c2724)

When we click on ```Origin domain``` the **www** domain under the dropdown menu is actually the wrong one. The correct one is located in our S3 bucket. 

![stg5 2x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/3576ea2d-f3b7-4ef4-9d2e-42aae779315b)

Head to S3: https://s3.console.aws.amazon.com/s3/home

Head to the **www** bucket. 

![stg5 3](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/26bcc914-71d1-4821-a923-09f6ba4a28a4)

Head to ```Properties```

![stg5 4](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/8a567404-1eb3-49ed-96be-676aec9c6a0f)

Scroll down to ```Static website hosting``` and copy the http link to your clipboard. Paste this into the origin domain under the Cloudfront distribution.

![stg5 4x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/fb09e134-c91a-4c76-9df0-651bab55198e)

Head back to Cloudfront: https://us-east-1.console.aws.amazon.com/cloudfront 

Paste the url to the Cloudfront distribution under ```Origin domain```

![stg5 5](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/91038048-4851-4d3e-a1de-776d7a28c933)

Change the ```Viewer``` to ```Redirect HTTP to HTTPS```.

![stg5 6x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/b9cf61fe-8798-46af-a2eb-48b13d315d74)

Under ```Alternate domain name (CNAME) -optional``` press on **Add item**. Type in the **www** url.

![stg5 7](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/2cbfc2fc-bb05-44a9-b17d-5ea23d79d160)

Under ```Custom SSL certificate -optional``` click on the dropdown menu and add the ACM certificates which should populate on its own.

![stg5 8](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/0630a110-bd3b-4f08-916f-c6e7a4af8b7d)

![stg5 10](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/1845814e-7f71-471f-93e6-aefc2e654fd8)

Under ```Web Application Firewall (WAF)``` click on ```Enable security protections``` and toggle ```Use monitor mode```.
Press on **Create Distribution**

![stg5 11](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/cb3846ae-955d-4357-8f52-a8a1b067cc03)

Leave the rest of the settings to default. Press on **Create distribution**.

![stg5 11x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/726e9fd9-2fcd-4420-9523-8a02367d6117)

![stg5 12](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/69b4d43c-9f50-4910-9847-751e0601b6bb)

We will now make a distribution for the **non-www**. 
Head back to S3: https://s3.console.aws.amazon.com/s3/home

Go to ```Buckets``` and click on the **non-www** bucket.

![stg5 13](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/cf9d5c88-7341-4f87-bfac-40dab771e9d0)

Head to ```Properties```. 

![stg5 14](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/c7e50e4b-4ae5-4ba2-b819-1b3a6151562c)

Scroll down to ```Static website hosting``` and copy the http link to your clipboard.

![stg5 15x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/fb5fdf62-15bb-4515-804e-9d5e9751b8aa)

On the same ```Static website hosting``` press on **Edit** 

![stg5 20y](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/c4de080e-a23d-47fd-8a64-30939937cf13)

Change the ```Protocol - Optional``` to ```https``` for the Cloudfront distribution.

![stg5 22](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/99d4c26b-7dc1-45ba-8162-3efb8d35d0ef)

Head back to Cloudfront: https://us-east-1.console.aws.amazon.com/cloudfront

Click on **Create Distribution**. In ```Origin domain``` paste the link below.

![stg5 15y](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/bdc8678f-753d-4da4-bc3a-bf7562b1d008)

Under ```Alternate domain name (CNAME) -optional``` enter in the **non-www** url.

![stg5 16](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/b5240356-32bb-4ae9-8de1-d0e935d5c950)

Under ```Custom SSL certificate -optional``` press on the dropdown menu and the SSL certificate should load in automatically.

![stg5 17](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/bb5f3b73-2c50-4112-8324-a9380843afb2)

Press on **Create Distribution**

![stg5 11x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/bff1c9ca-af4c-47c0-913c-cc2e34d38895)

You should have two distributions created. If you hover over to the ```Domain name``` and copy the cloudfront.net link you should be able to open the website with https.

![stg5 23x](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/09ad88c5-9367-4961-b7a3-eada0af51145)

If you go to the console on ```F12``` on your keyboard. You can see that the ```Server``` is labeled as ```AmazonS3``` with ```Via``` labled as ```Cloudfront```.
The website is currently secure but we need to change the Cloudfront distribution to our registered domain names instead.

![stg5 25](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/a8652a8b-2d54-46f1-8f14-c6a9fa5d581f)

Go to Route 53: https://us-east-1.console.aws.amazon.com/route53
Click on **Hosted Zone**.

![stg5 26](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/5eebf8b2-f9f7-4bac-9e2b-e929d733aba3)

Click on our **non-www** ```Hosted zone name```.

![stg5 27](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/202c8515-1f83-40a8-8189-07b108c09be3)

If you notice on records we have a total of six records now with the addition of the ```CNAME``` records. For now we need edit the ```A``` records. Click on the **www** with the ```A``` record and press on **Edit record**. 

![stg5 28](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/49d4760f-8633-4865-9075-cd15dca2a183)

Under ```Route traffic to``` select ```Alias to Cloudfront distribution``` and it should autocomplete with the Cloudfront distribution url. Select the url and press **Save**.

![stg5 29](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/4c8b7997-bbac-4e33-9973-e1b003c3e30e)

Do the same to the **non-www** and leave the ```Record name``` subdomain blank. Select ```Route traffic to``` ```Alias to Cloudfront distribution``` which should autofill for the cloudfront url for the **non-www** url. Select that and press **Save**.

![stg5 30](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/c53dcbb0-a62e-4a7a-bc3a-7cbbe5a15eab)

With that complete we can now test our website. Open a new tab and enter in the **www** domain. We can see that the connection is secure. If you dive deeper into the ```F12``` console you can see that  the ```Name``` is linked to the **www** domain and that the server is linked to ```Amazon S3``` ```Via``` ```Cloudfront```

![stg5 33](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/59ac1176-1a32-42fe-aff9-93881fdb5f58)

![stg5 34](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/431b15c7-53eb-4408-8b0d-59e829b59d24)

We can also test the **non-www** website and it should work too. The ```Name``` lists to the **non-www** but also lists the **www** url because it directs the website to the **www**. The ```Server``` is also set to ```AmazonS3``` ```Via``` ```Cloudfront```.

![stg5 32](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/37d71d41-b1af-42df-b684-876de2ed2008)

![stg5 35](https://github.com/Michael-DTran/React-Domain-Proj/assets/112426094/a9a00aab-9b72-4372-ba10-a8532c134450)

# Finished Links

www.awsreact.click




































