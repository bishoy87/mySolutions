**s3cmd: is a free open-source command-line tool and client for uploading, retrieving and managing data S3-compliant object storages. It’s a powerful tool for advanced users who are familiar with command-line programs but is also simple enough for beginners to learn quickly. It is also great for automation using, for example, shell scripts or cron.**

The Object Storage access keys can also be retrieved using the API request below. Replace the {uuid} with the unique identifier of your Object Storage device.

Request

```markdown
GET /1.3/object-storage/{uuid}

```
Response
```markdown
HTTP/1.0 201 OK
{
  "object_storage": {
    "uuid": "063881d4-edc5-4102-8214-9b7e11edb9cb",
    "name": "example-object-storage",
    "description": "example-object-storage",
    "zone": "nl-ams1",
    "url": "https://example-object-storage.nl-ams1.upcloudobjects.com/",
    "access_key": "UCOB7FIRRNAMAB58J1JO",
    "secret_key": "Z5x7HKkRxIQ8tzOoox7JIWqO79c011hENgNr/TAQ",
    "quota_gb": 500,
    "used_gb": 0,
    "state": "started",
    "created": "2020-07-27T07:36:45Z"
  }
}
```
Make note of your Object Storage’s endpoint url, access_key and sercret_key which are needed to be able to connect using an S3 client.

# Installing S3cmd
Thanks to its ease of automation, S3cmd is a good option for managing your Object Storage from your cloud server. You can install S3cmd on most Linux distributions using pip, the package installer for Python programming language.

First, check that Python is installed and available.
```console
# Ubuntu and Debian
sudo apt-get install python3 python3-distutils -y
# CentOS
sudo yum install python3 -y
```
Next, download the pip install script.

```console
wget https://bootstrap.pypa.io/get-pip.py -O ~/get-pip.py
```

Then install pip using the following command.
```console
sudo python3 ~/get-pip.py
```
Finally, install S3cmd using pip.

```console
sudo pip install s3cmd
```
That’s it! Now that S3cmd is installed, you’ll need to configure it to connect to your Object Storage. Continue below with the steps on how to accomplish this.

# Configuring S3cmd

Configure s3cmd by providing the access_key and secret_key of the user, pratima, which we created earlier in this chapter. Execute the following command and follow the prompts:
```console
s3cmd --configure
```
The s3cmd --configure command will create /root/.s3cfg.

Edit this file for the RGW host details. Modify host_base and host_bucket, as shown. Make sure these lines do not have trailing spaces at the end:

```console
sudo /root/.s3cfg
```
```markdown
host_base = rgw-node1.cephcookbook.com:8080 
host_bucket = ...
```