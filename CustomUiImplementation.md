# Custom UI Implementation
The Flutterwave SDK can be used with your custom UI where you handle the entire User Interface. This doc shows how to do so.

###  1. Create a `RaveNonUIManager` instance
Set the public key, encryption key and other required parameters.

        RaveNonUIManager raveNonUIManager =
                        new RaveNonUIManager()
                        .setAmount(amount)
                        .setCurrency(currency)
                        .setEmail(email)
                        .setfName(fName)
                        .setlName(lName)
                        .setNarration(narration)
                        .setPublicKey(publicKey)
                        .setEncryptionKey(encryptionKey)
                        .setTxRef(txRef)
                        .setPhoneNumber(phoneNumber, boolean)
                        .onStagingEnv(boolean)
                        .setMeta(List<Meta>)
                        .isPreAuth(boolean)
                        .setSubAccounts(List<SubAccount>)
                        .shouldDisplayFee(boolean)
                        .showStagingLabel(boolean)
                        .initialize();

<details>
    <summary>Function Definitions</summary>


| Function        | Parameter           | Type | Required  |
| ------------- |:-------------:| -----:| -----:|
| setAmount(amount)      |  This is the amount to be charged from card/account | `double` | Required
| setCurrency(currency) | This is the specified currency to charge the card in | `String` | Required
| setfName(fName) | This is the first name of the card holder or the customer  | `String` | Required
| setlName(lName) | This is the last name of the card holder or the customer | `String` | Required
| setEmail(email) | This is the email address of the customer | `String` | Required
| setNarration(narration) | This is a custom description added by the merchant. For `Bank Transfer` payments, this becomes the account name of the account to be paid into. See more details [here](https://developer.flutterwave.com/v2.0/reference#pay-with-bank-transfer-nigeria). | `String` | Not Required
| setPublicKey(publicKey) | Merchant's public key. Get your merchant keys here for [ staging](https://flutterwavedevelopers.readme.io/blog/how-to-get-your-staging-keys-from-the-rave-sandbox-environment) and [live](https://flutterwavedevelopers.readme.io/blog/how-to-get-your-live-keys-from-the-rave-dashboard)| `String` | Required
| setEncryptionKey(encryptionKey) | Merchant's encryption key. Get your merchant keys here for [ staging](https://flutterwavedevelopers.readme.io/blog/how-to-get-your-staging-keys-from-the-rave-sandbox-environment) and [live](https://flutterwavedevelopers.readme.io/blog/how-to-get-your-live-keys-from-the-rave-dashboard) | `String` | Required
| setTxRef(txRef) | This is the unique reference, unique to the particular transaction being carried out. It is generated by the merchant for every transaction | `String` | Required
| setPhoneNumber(phoneNumber) | This sets the customer's phone number. This functions is also overloaded to allow you specify whether the customer can edit their phone number as such: `setPhoneNumber(phoneNumber,false)`. When set to false, the user will not be able to change the number you set here.| `String`<br/><br/>Optional overloads:<br/>`String`, `boolean` | Not Required
| onStagingEnv(boolean) | Set to `true` if you want your transactions to run in the staging environment otherwise set to `false`. Defaults to false  | `boolean` | Not Required
| setMeta(`List<Meta>`) | Pass in any other custom data you wish to pass. It takes in a `List` of `Meta` objects | List<Meta> | Not Required
| setSubAccounts(`List<SubAccount>`) | Pass in a `List` of `SubAccount`,if you want to split transaction fee with other people. Subaccounts are your vendors' accounts that you want to settle per transaction. To initialize a `SubAccount` class, do `SubAccount(String subAccountId,String transactionSplitRatio)` or `SubAccount(String subAccountId,String transactionSplitRatio,String transactionChargeType, String transactionCharge)` to also charge the subaccount a fee. [Learn more about split payments and subaccounts](https://developer.flutterwave.com/docs/split-payment).| `List<SubAccount>`| Not Required
| setIsPreAuth(boolean) | Set to `true` to preauthorise the transaction amount. [Learn more about preauthourization](https://developer.flutterwave.com/v2.0/reference#introduction-1). | `int` | Not Required
| setPaymentPlan(payment_plan) | If you want to do recurrent payment, this is the payment plan ID to use for the recurring payment, you can see how to create payment plans [here](https://flutterwavedevelopers.readme.io/v2.0/reference#create-payment-plan) and [here](https://flutterwavedevelopers.readme.io/docs/recurring-billing). This is only available for card payments | `String` | Not Required
| initialize() | Returns the prepared `RaveNonUiManager` that can be used with any of the payment managers for the different payment methods   |  N/A | Required
</details>

###  2. Initiate a charge
Use the payment method's manager to initiate a charge. For example to charge cards, use the `CardPaymentManager`. (Store the Payment Manager object in a variable to be able to access it in the next steps).

       CardPaymentManager cardPayManager = new CardPaymentManager(
       raveNonUIManager, CardPaymentCallback, SavedCardsListener);
       Card card = new Card(
                        "5531886652142950")
                        "12",
                        "22",
                        "123"
                ); // Test MasterCard PIN

        cardPayManager.chargeCard(card);

###  3. Implement callbacks
We have predefined callback classes for each payment method. Make your Fragment/Activity (or other class handling the payment interaction) implement the specified callback for the payment method you'd like to use, and override the specified callback functions. For example to charge cards, implement the `CardPaymentCallback`
<details>
  <summary>See Example</summary>


  ```java
    public class PaymentActivity extends AppCompatActivity implements CardPaymentCallback {

    // Step 1 and Step 2 here...

    // Methods to override for card payment
    @Override
    public void collectCardPin() {
        //collect PIN from User and pass to the CardPayManager object.
        cardPayManager.submitPin(pin);
    }

    @Override
    public void collectOtp(String message) {
        //collect OTP from User and pass to the CardPayManager object.
        cardPayManager.submitOtp(otp);
    }

    @Override
    public void collectAddress() {
        //collect Card Address details from User and pass to the CardPayManager object.
        cardPayManager.submitAddress(address);
    }

    @Override
    public void showAuthenticationWebPage(String authenticationUrl) {
        //Show a Webview for transaction authentication and call the CardPayManager object when the webview authentication is done. 
        // See the JavaDoc for this method for more details.
        
        cardPayManager.onWebpageAuthenticationComplete();
    }

    @Override
    public void onError(String errorMessage, @Nullable String flwRef) {
        // Error during transaction
        Toast.makeText(this, errorMessage, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onSuccessful(String flwRef) {
        // Transaction successful
        Toast.makeText(this, "Transaction Successful", Toast.LENGTH_LONG).show();
    }
}

```

</details>

<details>
  <summary>See All Payment Managers and Callbacks</summary>


  ## Payment Methods and corresponding PaymentManager and Callback class names
  | Payment Method        | Payment Manager Class Names           | Payment Callback Class Names |
  | ------------- |:-------------:| -----:|
  | Account (Direct Debit)      |  `AccountPaymentManager` | `AccountPaymentCallback`
  | ACH      |  `AchPaymentManager` | `AchPaymentCallback`
  | Bank Transfer      |  `BankTransferPaymentManager` | `BankTransferPaymentCallback`
  | Barter      |  `BarterPaymentManager` | `BarterPaymentCallback`
  | Card      |  `CardPaymentManager` | `CardPaymentCallback`
  | Francophone Mobile Money      |  `FrancophoneMobileMoneyPaymentManager` | `FrancophoneMobileMoneyPaymentCallback`
  | Ghana Mobile Money      |  `GhMobileMoneyPaymentManager` | `GhMobileMoneyPaymentCallback`
  | MPesa      |  `MpesaPaymentManager` | `MpesaPaymentCallback`
  | Rwanda Mobile Money      |  `RwfMobileMoneyPaymentManager` | `RwfMobileMoneyPaymentCallback`
  | South Africa Bank Account      |  `SaBankAccountPaymentManager` | `SaBankAccountPaymentCallback`
  | Uganda Mobile Money      |  `UgandaMobileMoneyPaymentManager` | `UgandaMobileMoneyPaymentCallback`
  | UK Bank Payment      |  `UkBankPaymentManager` | `UkBankPaymentCallback`
  | USSD      |  `UssdPaymentManager` | `UssdPaymentCallback`
  | Zambia Mobile Money      |  `ZmMobileMoneyPaymentManager` | `ZmMobileMoneyPaymentCallback`
</details>

>  We worked on a module to simplify charge verification when using the No-UI approach. You can read about using it [here](ChargeVerificationUtils.md)

> Due to the multiple payment methods available, all the methods of the different payment callback classes could not be documented here. Please refer to the individual Javadocs in the code if clarification is needed.

>  To see a more practical way of using the sdk, head to our sample app in the repository [here](https://github.com/Flutterwave/rave-android/tree/master/app)

## Configuring Proguard
To configure Proguard, add the following lines to your proguard configuration file. These will keep files related to this sdk
```
keepclasseswithmembers public class com.flutterwave.raveandroid.** { *; }
dontwarn com.flutterwave.raveandroid.card.CardFragment
```


## Extras
### Fee checking
If you would like to include fee checks, make your calling class implement the `FeeCheckListener`, then pass the calling class as an argument to the Payment Manager's `fetchTransactionFee()` function.

<details>
  <summary>Example</summary>


  ```java
public class PaymentActivity extends AppCompatActivity
        implements FeeCheckListener // Implement if you want to be able to check fees beforehand
        // Don't forget to implement the payment callback(s) for the payment method(s) you want to use.
{

    //...
    //...
    void fetchFee(){
        // Call the fetch transaction  fee function with `this` as an argument. The overridden callbacks will then be called as applicable.
        cardPayManager.fetchTransactionFee(this);
    }

    // Methods to override
    @Override
    public void onTransactionFeeFetched(String chargeAmount, String fee) {
        // Display the fee to the customer then proceed to payment at your 
        // discretion using cardPayManager.chargeCard(card);
    }

    @Override
    public void onFetchFeeError(String errorMessage) {
        //Display error to user, or  proceed  with payment at your discretion
    }

}

```

</details>

### Card Save Feature
For Card transactions, if you would like to implement the card saving feature, make your calling class implement the `SavedCardListener`, then pass `this` as an argument to the constructor of `CardPaymentManager`. You can also set the savedCardListener using `CardPaymentManager.setSavedCardsListener(this)`.
* To save a card, call `CardPaymentManager.saveCard()` after a card payment is successful (before starting another payment).
* To fetch the user's saved cards, use `CardPaymentManager.fetchSavedCards(boolean)`.
* To delete a saved card, use `CardPaymentManager.deleteSavedCard(cardHash)`.


<details>
  <summary>Example</summary>


  ```java
public class PaymentActivity extends AppCompatActivity implements SavedCardsListener, CardPaymentCallback 
{  
    //...
    //...
    
    // To save  a card call `CardPaymentManager.saveCard()` after a card payment is successful (before starting another payment).
    
    /**
     * Fetch the user's saved cards. The overridden functions will be called as applicable.
     */
    void fetchUserSavedCards(){
        cardPayManager.fetchSavedCards();
    }
    
    /**
     * Fetch the user's saved cards. The overridden functions will be called as applicable.
     */
    void deleteSavedCard(SavedCard card){
        cardPayManager.deleteSavedCard(card.getCardHash());
    }

    // Methods to override
    @Override
    public void onSavedCardsLookupSuccessful(List<SavedCard> cards, String phoneNumber) {
        // Check that the list is not empty, show the user to select which they'd like to charge, then proceed to 
        // charge the saved card using cardPayManager.chargeSavedCard(savedCard)
    }

    @Override
    public void onSavedCardsLookupFailed(String message) {
        // Saved card lookup failed. 
    }

    @Override
    public void collectOtpForSaveCardCharge() {
        // Collect otp and pass to the payment manager using cardPayManager.submitOtp()
    }

    @Override
    public void onCardSaveSuccessful(String phoneNumber) {
        //card saved successful
    }

    @Override
    public void onCardSaveFailed(String message) {
        //Unable to save card
    }

    @Override
    public void onDeleteSavedCardRequestSuccessful() {
        // Card deleted successfully
    }

    @Override
    public void onDeleteSavedCardRequestFailed(String message) {
        //Unable to delete card
    }

}

```

</details>
