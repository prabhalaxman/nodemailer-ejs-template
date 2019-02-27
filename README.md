# nodemailer-ejs-template
Here is code snipet to render html file using ejs and send as template using nodemailer
refer the sample.html file 

Sample code snipet to send mail using nodemailer


try {

        var acceptUrl = "http://localhost:3000/hbs_sample/" + userId + "/" + purchaseId
        var rejectUrl = "http://localhost:3000/hbs_sample/" + userId + "/" + purchaseId
        var user = {
            userName: "Jack",
            userCategory: 'Captain',
            shipName: 'Test ship',
            firstName: 'John', lastName: 'Doe', data: [
                { name: "Xyz ", type: "Oil1", grade: "DMA", qty: "10", price: "1000" },
                { name: "Xyz ", type: "Oil2", grade: "DMC", qty: "20", price: "3000" },
                { name: "Xyz ", type: "Oil3", grade: "DMX", qty: "10", price: "2000" },
            ],
            acceptUrl: acceptUrl,
            rejectUrl: rejectUrl
        };
        var subject = ejs.render('Hello <%= firstName %>', user);


        var templateString = fs.readFileSync('html file path', 'utf-8');

        // var template = hbs.compile(templateString);

        var text = ejs.render(templateString, user);

        var transporter = nodemailer.createTransport({
            service: 'Gmail',
            auth: {
                user: '<Your gmail>',
                pass: '<Password>'
            }
        });

        var maillist = [
            'xxxxxx@gmail.com',
            'yyyyy@gmail.com'
          ];

        // Message object
        let message = {
            from: 'from mail id',
            // Comma separated list of recipients
            to: maillist,

            // Subject of the message
            subject: subject,

            // plaintext body
            // text: template("http:localhost:3000"),
            html: text
          
        };

        console.log('node mailer msg ' + message)
        transporter.sendMail(message, (error, info) => {
            if (error) {
                console.log('send mail error ' + error)
                return process.exit(1);
            }
            console.log('mail msg ' + message)
            // only needed when using pooled connections
            transporter.close();
        });


    } catch (err) {
        // writeErrorInLogFile("Error Log mail sending ", errorLog, config.error);
        // reject(err);
        // res.json({ "status": "Failure", "Message": "Failed to update Port Info" });
        console.log('send mail error ' + err)
    } finally {
    }








