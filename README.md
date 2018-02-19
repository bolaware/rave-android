# Rave's Android Drop In UI

Rave's Android Drop-In is a readymade UI that allows you to accept card and bank payments in your Android app.

<img alt="Screenshot of Drop-In" src="https://firebasestorage.googleapis.com/v0/b/saveup-9e594.appspot.com/o/Group.png?alt=media&token=e0c89192-b2a4-47e0-a883-3a78005acd2a" width="600"/>

## Requirements


The minimum supported SDK version is 15

## Adding it to your project


**Step 1.** Add it in your root build.gradle at the end of repositories:

    allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}

**Step 2.** Add the dependency

    dependencies {
	     compile 'com.github.Flutterwave:rave-android:1.0.14'
	}
**Step 3.** Add the required permission

Add the `READ_PHONE_PERMISSION` permission to your android manifest

     <uses-permission android:name="android.permission.READ_PHONE_STATE"/>


> **REQUIRED PERMISSION**
> This library requires the **READ_PHONE_PERMISSION** to get the build number for fraud detection and flagging as recommended here https://developer.android.com/training/articles/user-data-ids.html#i_abuse_detection_detecting_high_value_stolen_credentials

## Usage

###  1. Create a `RavePayManager` instance
Set the public key, private key and other required parameters. The `RavePayManager` accepts a mandatory instance of  the calling `Activity`

        new RavePayManager(activity).setAmount(amount)
                        .setCountry(country)
                        .setCurrency(currency)
                        .setEmail(email)
                        .setfName(fName)
                        .setlName(lName)
                        .setNarration(narration)
                        .setPublicKey(publicKey)
                        .setSecretKey(secretKey)
                        .setTxRef(txRef)
                        .acceptAccountPayments(boolean)
                        .acceptCardPayments(boolean)
                        .onStagingEnv(boolean)
                        .allowSaveCardFeature(boolean)
                        .setMeta(List<Meta>)
                        .withTheme(styleId)
                        .initialize();
* `setAmount()` - This is the amount to be charged from card/account. `double`
* `setCountry()` - This is the route country for the transaction with respect to the currency. `String`
* `setCurrency` - This is the specified currency to charge the card in. `String`
* `setEmail()` - This is the email address of the customer. `String`
* `setfName()` - This is the first name of the card holder or the customer. `String`
* `setlName()` - This is the last name of the card holder or the customer. `String`
* `setNarration()` - This is a custom description added by the merchant. `String`
* `setPublicKey()` - Merchant's public key `String`
* `setSecretKey()` - Merchant's secret key `String`
* `setTxRef()` - This is the unique reference, unique to the particular transaction being carried out. It is generated by the merchant for every transaction `String`
* `acceptAccountPayments()` - Set to `true` if you want to accept payments via bank accounts, else set to false. `boolean`. Defaults to `true`
* `acceptCardPayments()` - Set to `true` if you want to accept payments via cards, else set to `false`. `boolean`. Defaults to `true`
* `onStagingEnv()` - Set to `true` if you want your transactions to run in the staging environment otherwise set to `false`. Defaults to `false`
* `allowSaveCardFeature` - Set to `true` if you want your users to be able to save their cards for later reuse otherwise set to `false`.
* `setMeta()` - Pass in any other custom data you wish to pass. It takes in a `List` of `Meta` objects
* `withTheme()` - Sets the theme of the UI. `int`
* `initialize` - Launch the Rave Payment UI

> **SECURITY ALERT**
> You should never store your **SECRET KEY** on the user's device

###  2. Handle the response
In the calling activity, override the `onActivityResult` method to receive the payment response as shown below

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == RaveConstants.RAVE_REQUEST_CODE && data != null) {
            String message = data.getStringExtra("response");
            if (resultCode == RavePayActivity.RESULT_SUCCESS) {
                Toast.makeText(this, "SUCCESS " + message, Toast.LENGTH_SHORT).show();
            }
            else if (resultCode == RavePayActivity.RESULT_ERROR) {
                Toast.makeText(this, "ERROR " + message, Toast.LENGTH_SHORT).show();
            }
            else if (resultCode == RavePayActivity.RESULT_CANCELLED) {
                Toast.makeText(this, "CANCELLED " + message, Toast.LENGTH_SHORT).show();
            }
        }
        else {
            super.onActivityResult(requestCode, resultCode, data);
        }
    }
The intent's `message` object contains the raw JSON response from the Rave API. This can be parsed to retrieve any additional payment information needed.

###  3. Customize the look
You can apply a new look by changing the color of certain parts of the UI to highlight your brand colors

    <style name="DefaultTheme" parent="AppTheme.NoActionBar">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="OTPButtonStyle">@style/otpBtnStyle</item>
        <item name="PayButtonStyle">@style/payBtnStyle</item>
        <item name="PinButtonStyle">@style/pinButtonStyle</item>
        <item name="OTPHeaderStyle">@style/otpHeaderStyle</item>
        <item name="TabLayoutStyle">@style/tabLayoutStyle</item>
        <item name="PinHeaderStyle">@style/pinHeaderStyle</item>
        <item name="SavedCardButtonStyle">@style/svdCardsBtnStyle</item>
    </style>

##  Help
* Have issues integrating? Join our [Slack community](https://join.slack.com/t/flutterwavedevelopers/shared_invite/enQtMjU2MjkyNDM5MTcxLWFlOWNlYmE5MTIxNjAwYzc5MDVjZjNhYTJjNTA0ZTQyNDJlMDhhZjJkN2QwZGJmNWMyODhlYjMwNGUyZDQxNTE) for support
* Find a bug? [Open an issue](https://github.com/Flutterwave/rave-android/issues)
* Want to contribute? [Check out contributing guidelines]() and [submit a pull request](https://help.github.com/articles/creating-a-pull-request).

## Want to contribute?

Feel free to create issues and pull requests. The more concise the pull requests the better :)


## License

```
Rave's Android DropIn UI
MIT License

Copyright (c) 2017

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```