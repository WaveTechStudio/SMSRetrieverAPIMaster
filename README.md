# Automatic SMS Verification with the SMS Retriever API
As per Google's new policy with the SMS Retriever API, you can perform SMS-based user verification in your Android app automatically, without requiring the user to manually type verification codes, and without requiring any extra app permissions.  

## Google's warning
<img src="./screens/googles_warning.png" width=“400”/>

## Solution
<img src="./screens/sms_retriever_api.png" width=“400”/>

## Work flow

- Enter or pick mobile number from smart lock hint slector and intiate SMS verification call to your server.
- App request your server to verify the entered mobile number.
- Your app calls the SMS Retriever API at the same time and listening for an SMS from your server.
- Your server sends an SMS message that includes a verification code and a hash to identify your app.
- When user's device receives the SMS message, SMS Retriever API reads the SMS in your app.
- App extract verification code from SMS and sends to your server for code verification.
- Your server receives the verification code and after validating it can return success response to proceed.

## Prerequisites
The SMS Retriever API is available only on Android devices with Play services version 10.2 and newer.

## Important
The standard SMS format is given blow.

    <#> Your ExampleApp code is: 123ABC78 
    FA+9qCX9VSu

SMS alwayse starts with <#> sign and have a hash key FA+9qCX9VSu to identify your app it is generated with your app's package id. You just need to get this has key from app and share with your server. 
In next few steps you will see how to create hash keys.

### Dependencies used
    // Add at app level gradle file
    implementation 'com.google.android.gms:play-services-base:16.0.1'
    implementation 'com.google.android.gms:play-services-identity:16.0.0'
    implementation 'com.google.android.gms:play-services-auth:16.0.1'
    implementation 'com.google.android.gms:play-services-auth-api-phone:16.0.0'
    // Add at project level gradle file
    classpath 'com.google.gms:google-services:4.2.0'
    
## Integration steps
1. App's hash key would be like this qzwS5M4KQ5H. In this sample code AppSignatureHashHelper class is responsible to get Hash key of associated app packege.
       
       Inside Main Activity 
       Log.d(TAG, "Apps Hash Key: " + appSignatureHashHelper.getAppSignatures().get(0));
       Apps Hash Key: qzwS5M4KQ5H
        
                
2. You can add any dummy credentials on login screen to proceed View communicates to presenter.

        <receiver
            android:name=".SMSReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="com.google.android.gms.auth.api.phone.SMS_RETRIEVED" />
            </intent-filter>
        </receiver> 
3. Create SMSReceiver class that will listen SMS and extract code and create OTPReceiveListener that will communicate with Activities/Fragments.
      
       public interface OTPReceiveListener {
           void onOTPReceived(String otp);
           void onOTPTimeOut();
           void onOTPReceivedError(String error);
           }
       }
4.  Create SMSReceiver listenrs and Intitiate SmsRetrieverClient. 

        private void startSMSListener() {
        try {
            smsReceiver = new SMSReceiver();
            smsReceiver.setOTPListener(this);

            IntentFilter intentFilter = new IntentFilter();
            intentFilter.addAction(SmsRetriever.SMS_RETRIEVED_ACTION);
            this.registerReceiver(smsReceiver, intentFilter);

            SmsRetrieverClient client = SmsRetriever.getClient(this);
            Task<Void> task = client.startSmsRetriever();
            task.addOnSuccessListener(new OnSuccessListener<Void>() {
                @Override
                public void onSuccess(Void aVoid) {
                    // API successfully started
                }
            });
            task.addOnFailureListener(new OnFailureListener() {
                @Override
                public void onFailure(@NonNull Exception e) {
                    // Fail to start API
                }
            });
        } catch (Exception e) {
            e.printStackTrace();
          }
        }
5. You will receive OTP in call back methods implemented in you  Activity/Fragment.
    
       @Override
       public void onOTPReceived(String otp) {
       }

       @Override
       public void onOTPTimeOut() {
       }

       @Override
       public void onOTPReceivedError(String error) {
       }

## How to run a sample
- Clone or download the project open it with Android Studio compile and run it will work.


### Server Side Implementation / SMS Guide
[Google's official doc](https://developers.google.com/identity/sms-retriever/verify)

## Author
[Waheed Nazir (WaveTechStudio)](https://www.linkedin.com/in/waheed-nazir-36521579/ "Waheed Nazir (WaveTechStudio)")

## License
It is totally free to use. :)

## Credits
[Google's official doc](https://developers.google.com/identity/sms-retriever/overview)
[Chintan Desai's (Repo/Kotlin)](https://github.com/chintandesai49/SMSRetrieverAPIDemo) 
