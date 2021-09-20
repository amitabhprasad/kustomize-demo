### Install steps
- create namespace `ibmrpa`
- Create pull secret
    - `ibmrpa-pull-secret`
        - `cp.icr.io `
        - `docker.io`
        - `cp.stg.icr.io`
        - `hyc-rpa-team-docker-local.artifactory.swg-devops.com`


###
```
### Create SMTP secret
cat <<EOF | oc apply -f -
kind: Secret
apiVersion: v1
metadata:
  name: rpa-smtp-rpa-sample
  namespace: ${RPA_PROJECT}
data:
  password: YmxhaA==
  username: YmxhaA==
type: Opaque
EOF
​
cat <<EOF | oc apply -f -
kind: Secret
apiVersion: v1
metadata:
  name: rpa-db-rpa-sample
  namespace: ${RPA_PROJECT}
data:
  AddressContext: >-
    RGF0YSBTb3VyY2U9cnBhLXNhbXBsZS1kYi5pYm1ycGEuc3ZjXFNRTEVYUFJFU1MsMTQzMztJbml0aWFsIENhdGFsb2c9YWRkcmVzcztVc2VyIElEPWNpY2Q7UGFzc3dvcmQ9d2RnMjAyMGlibTtDb25uZWN0IFRpbWVvdXQ9MzA7RW5jcnlwdD1GYWxzZTtUcnVzdFNlcnZlckNlcnRpZmljYXRlPUZhbHNlO0FwcGxpY2F0aW9uSW50ZW50PVJlYWRXcml0ZTtNdWx0aVN1Ym5ldEZhaWxvdmVyPUZhbHNl
  AutomationContext: >-
    RGF0YSBTb3VyY2U9cnBhLXNhbXBsZS1kYi5pYm1ycGEuc3ZjXFNRTEVYUFJFU1MsMTQzMztJbml0aWFsIENhdGFsb2c9YXV0b21hdGlvbjtVc2VyIElEPWNpY2Q7UGFzc3dvcmQ9d2RnMjAyMGlibTtDb25uZWN0IFRpbWVvdXQ9MzA7RW5jcnlwdD1GYWxzZTtUcnVzdFNlcnZlckNlcnRpZmljYXRlPUZhbHNlO0FwcGxpY2F0aW9uSW50ZW50PVJlYWRXcml0ZTtNdWx0aVN1Ym5ldEZhaWxvdmVyPUZhbHNl
  KnowledgeBase: >-
    RGF0YSBTb3VyY2U9cnBhLXNhbXBsZS1kYi5pYm1ycGEuc3ZjXFNRTEVYUFJFU1MsMTQzMztJbml0aWFsIENhdGFsb2c9a25vd2xlZGdlO1VzZXIgSUQ9Y2ljZDtQYXNzd29yZD13ZGcyMDIwaWJtO0Nvbm5lY3QgVGltZW91dD0zMDtFbmNyeXB0PUZhbHNlO1RydXN0U2VydmVyQ2VydGlmaWNhdGU9RmFsc2U7QXBwbGljYXRpb25JbnRlbnQ9UmVhZFdyaXRlO011bHRpU3VibmV0RmFpbG92ZXI9RmFsc2U=
  WordnetContext: >-
    RGF0YSBTb3VyY2U9cnBhLXNhbXBsZS1kYi5pYm1ycGEuc3ZjXFNRTEVYUFJFU1MsMTQzMztJbml0aWFsIENhdGFsb2c9d29yZG5ldDtVc2VyIElEPWNpY2Q7UGFzc3dvcmQ9d2RnMjAyMGlibTtDb25uZWN0IFRpbWVvdXQ9MzA7RW5jcnlwdD1GYWxzZTtUcnVzdFNlcnZlckNlcnRpZmljYXRlPUZhbHNlO0FwcGxpY2F0aW9uSW50ZW50PVJlYWRXcml0ZTtNdWx0aVN1Ym5ldEZhaWxvdmVyPUZhbHNl
type: Opaque
EOF
```