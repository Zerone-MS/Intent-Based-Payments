# CardsPe Intent based Payments (Android)

## Registration

To register yourself as enterprise merchant on CardsPe, you have to contact our onboarding team. Drop an email to <sales@cards.pe> .

## Obtain API keys

After onboarding you will receive your API keys on your registered primary email address. You will also receive details related to sandbox environment like {baseUrl}.
The API keys consists {clientId} & {clientSecret}.

## Integration

The integration is divided into two parts, the first part consists API integration and the other part consists few lines of code in mobile app.
Please follow below steps for complete integration :

#### Step 1 : Create an Order

##### Endpoint : <https://{clientId}:{clientSecret}@{baseUrl}/pos/v3/enterprise/order/create>

##### Type : POST

##### Request Body :

```json
{
  "amount": 1000, //Note : Amount is in paise
  "orderId": "12345678901234567689001", // Note : Optional field
  "paymentMethods": ["CARD", "QR"]
}
```

##### Response Body :

```json
{
  "statusCode": "10000",
  "message": "order created successfully",
  "data": {
    "amount": "10.00",
    "currency": "INR",
    "payeeId": "6049e5e734432a0bc5cd47c9",
    "orderId": "12345678901234567689001",
    "createdAt": "2021-11-01T09:05:23.315Z"
  }
}
```

#### Step 2 : Use above Order details to invoke Intent from android APP, Create Activity Launcher to start any activity for result:

```java
      ActivityResultLauncher<Intent> mActivityResultLauncher = registerForActivityResult(
                new ActivityResultContracts.StartActivityForResult(),
                new ActivityResultCallback<ActivityResult>() {
                    @Override
                    public void onActivityResult(ActivityResult result) {
                        if (result.getResultCode() == Activity.RESULT_OK) {
                            // Here, no request code
                            Intent data = result.getData();
                            String cardspeTxnStatus= data.getStringExtra("status")
                            //doSomeOperations();
                        } } });

```

#### Step 3 : Using mActivityResultLauncher to open CardsPe using Intent:

```java
 Intent intent = new Intent(“com.cards.pe”)
 intent.putExtra(“orderId”, ORDER_ID)
 intent.putExtra("keyId", keyID)
 mActivityResultLauncher.launch(intent);

```

#### \*Prerequisites :

##### To check if CardsPe app is installed, In your Android.Manifest file add cardspe package name in queries :

```xml
    <queries>
        <package android:name="com.zup.merchant"/>
        <package android:name="com.zup.merchant.debug"/>
    </queries>

```

##### Code to check if CardsPe app is installed or not :

```java
     private boolean isAppInstalled(String packageName) {
        try {    this.getPackageManager().
         getPackageInfo(packageName,PackageManager.GET_ACTIVITIES);
            return true;
         } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
            return false;
         }

     }

```

### Note :

#### 1. Transaction Status check API (To be updated)

#### 2. Webhook integration available (Please ask from your Account Manager)

## License

All Rights Reserved

Copyright (c) Zerone Microsystems Private Limited

Created by
Team CardsPe

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
