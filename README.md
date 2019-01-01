# Automatic SMS Verification with the SMS Retriever API
As per Google's new policy with the SMS Retriever API, you can perform SMS-based user verification in your Android app automatically, without requiring the user to manually type verification codes, and without requiring any extra app permissions.  

## Google's warning
<img src="./screens/googles_warning.PNG" width=“400”/>

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

## Description

- MainActivity implements MainActivityPresenter.View and overrides these methods.

        public interface View {
          void updateLoginResponse(String response);
          void clearInputFeilds();
          void showProgressBar();
          void hideProgressBar();
        }
                
- You can add any dummy credentials on login screen to proceed View communicates to presenter.

        private MainActivityPresenter presenter = new MainActivityPresenter(this);
        presenter.loginAPICall("email", "password");  
   after calling this there would be a dummy loader for fake API call (see MainActivityPresenter class) and return response to View. 
   
       mainActivityView.updateLoginResponse("Successfully logged in with these credentials: \n\n" + user.toString());
       mainActivityView.clearInputFeilds();
       mainActivityView.hideProgressBar();


# [Architecture used](https://github.com/googlesamples/android-architecture "Architecture used")

![](https://i.imgur.com/5fg5z5r.png)


### Dependencies used

    //App's dependencies
    implementation 'com.android.support:appcompat-v7:28.0.0'

### Presenter class for communication

    public class MainActivityPresenter {
    private User user;
    private View mainActivityView;

    public MainActivityPresenter(View mainActivityView) {
        this.user = new User();
        this.mainActivityView = mainActivityView;
    } 

    public void loginAPICall(String email, String password) {
        user.setEmail(email);
        user.setPassword(password);

        //Do API call here, I am adding dummy loader/for delay
        mainActivityView.showProgressBar();

        new CountDownTimer(3000, 1000) {

            public void onTick(long millisUntilFinished) {
            }

            public void onFinish() {

                mainActivityView.updateLoginResponse("Successfully logged in with these credentials: \n\n" + user.toString());

                mainActivityView.clearInputFeilds();
                mainActivityView.hideProgressBar();
            }
        }.start();
    }
      public interface View {

         void updateLoginResponse(String response);

          void clearInputFeilds();

         void showProgressBar();

         void hideProgressBar();
      }
    }


## How to run a sample
- Clone or download the project open it with Android Studio compile and run it will work.


### Images
<img src="./screens/1.png" width="200"/> <img src="./screens/2.png" width="200"/>
 <img src="./screens/3.png" width="200"/>
<br/>


## Author
[Waheed Nazir (GreenProLogix)](https://www.linkedin.com/in/waheed-nazir-36521579/ "Waheed Nazir (GreenProLogix)")

## License
It is totally free to use. :)
