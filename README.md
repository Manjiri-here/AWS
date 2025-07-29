1) Create s3 bucket
   manjiri@Abhis-MacBook-Pro ~ % aws s3api create-bucket \
    --bucket assignment-demo-s3bucket \
    --region us-east-1

{
    "Location": "/assignment-demo-s3bucket"
}



<img width="1792" height="519" alt="Screenshot 2025-07-29 at 9 00 25 PM" src="https://github.com/user-attachments/assets/5cba5c22-3156-4c21-aa13-a6de0b0744b3" />



2) Delete s3 bucket
manjiri@Abhis-MacBook-Pro ~ % aws s3api delete-bucket \
    --bucket assignment-demo-s3bucket \
    --region us-east-1

