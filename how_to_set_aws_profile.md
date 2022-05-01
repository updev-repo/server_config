## How to set aws profile

+ add profile credential
    + Here we are setting access key and secret key for each profile
    + `sudo nano ~/.aws/credentials`
        + ```
            [default]
            aws_access_key_id=AKIAIOSFODNN7EXAMPLE
            aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

            [profile1]
            aws_access_key_id=AKIAIOSFODNN7EXAMPLE
            aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

            [profile2]
            aws_access_key_id=AKIAIOSFODNN7EXAMPLE
            aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
            ```
            
+ add profile config
    + Here we are setting region for each profile
    + `sudo nano ~/.aws/credentials`
        + ```
            [default]
            region = us-west-2

            [profile1]
            region = us-west-1

            [profile2]
            region = us-west-2
            ```
          
+ Set current profile and view
    + `export AWS_PROFILE=profile1`  
    + `aws configure list`
    + `aws configure list-profiles`
            
        
