### Create an S3 bucket and go to permission in the bucket policy paste the code 
```yml
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::gitbucket-static/*"   (change the bucket name here)
        }
    ]
}
### Go to properties and configure static hosting --> enable it --> in index document give name index.html
### Create a new repository in your GitHub account  
###click on settings --> Select Secrets and Variables from settings, and select Actions
Select New repository secret.
Enter AWS_ACCESS_KEY_ID 
Enter AWS_SECRET_ACCESS_KEY

### create a file .github/workflows/main.yml
```yml
name: Upload Website

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v1

    - name: Configure AWS CredentialS
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ap-south-1

    - name: Deploy static site to S3 bucket
      run: 
        aws s3 sync . s3://swiggymybucket --delete  (change the bucket name and remove this comment)
```

check the file is running on github
and then make a new file in the repo  --> index.html paste the code in that file
```yml


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Simple Website</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }
        header {
            background-color: #333;
            color: #fff;
            padding: 1rem;
            text-align: center;
        }
        nav {
            margin: 1rem;
            text-align: center;
        }
        nav a {
            margin: 0 1rem;
            text-decoration: none;
            color: #333;
        }
        main {
            padding: 2rem;
        }
        footer {
            background-color: #333;
            color: #fff;
            text-align: center;
            padding: 1rem;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to My Simple Website</h1>
    </header>
    <nav>
        <a href="#home">Home</a>
        <a href="#about">About</a>
        <a href="#contact">Contact</a>
    </nav>
    <main>
        <h2>Home</h2>
        <p>This is a simple HTML website example. You can add more content here.</p>
    </main>
    <footer>
        <p>© 2024 My Simple Website</p>
    </footer>
</body>
</html>

### then go to s3 properties and click on static hosting url and run it
