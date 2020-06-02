# okra-node
> This is a npm module that abstracts the complexity of using okra with node.

## USAGE

### 1. Install the module

Install with `yarn`:

```bash
yarn add okra-node
```

_but you can use `npm` if you like:_

```bash
npm install --save okra-node
```

### 2. Import the module
In your `app.js` or any module where the component would be used:

```node
import * as okra_client from "okra-node";
```

### 3. Implementation
Okra node exposes a bunch of APIs that might be useful in your application.

* **getAuth**: get all successful bank verification for customer
  ```node
    okra_client.getAuth(accessToken, {}, (err, results) => {
	// Handle err
    const auths = results.auths;
    });
  ```
* **getTransactions**: get all transactions associated to your bank
  ```node
    okra_client.getTransactions(accessToken, {}, (err, results) => {
	// Handle err
    const transactions = results.trans;
    });
  ```
* **getBalances**: this returns the real-time balance for each of an Record's accounts
  ```node
    okra_client.getBalances(accessToken, {}, (err, results) => {
	// Handle err
    const balances = results.balances;
    });
  ```
* **getIdentities**: this returns all the identities of customer related to your company account
  ```node
    okra_client.getIdentities(accessToken, {}, (err, results) => {
	// Handle err
    const identities = results.identities;
    });
  ```
* **getRecords**: this returns all the records of transaction
  ```node
    okra_client.getRecords(accessToken, {}, (err, results) => {
	// Handle err
    const records = results.records;
    });
  ```
* **retryRecord**: this re-run a record
  ```node
    okra_client.retryRecord(accessToken, {record_id: string, user: string}, (err, results) => {
	    // Handle err
    });
  ```
* **getAccounts**: this returns all the accounts of customer associated to your company
  ```node
    okra_client.getAccounts(accessToken, {}, (err, results) => {
	    // Handle err
    const accounts = results.accounts;
    });
  ```
* **getTotalDebitCredits**: this returns the total credit and debits made on a customer account associated to your company.
  ```node
    okra_client.getTotalDebitCredits(accessToken, {account:"5e1efdsa842182515cedd066"}, (err, results) => {
	    // Handle err
    const total = results.result
    });
  ```
Field | Required | Description
---|---|---
**account**<br>`String` | yes | Id of the said account.

* **getProducts**: this returns all the available products
  ```node
    okra_client.getProducts(accessToken, {}, (err, results) => {
	    // Handle err
    const product = results.products;
    });
  ```
  
* **getProductsByRecord**: retrieve either AUTH, BALANCE, TRANSACTIONS, IDENTITY, INCOME of a customer using record Id.
  ```node
    okra_client.getProductsByRecord(accessToken, {}, (err, results) => {
	    // Handle err
    const product = results;
    });
  ```

* **getRecordByMethod**: this returns all the available products
  ```node
    okra_client.getRecordByMethod(accessToken, { record: 'record_id', method: 'okra_product' }, (err, results) => {
	    // Handle err
    const product = results['okra_product'];
    });
  ```

* **getProductsByCustomer**: to retrieve either AUTH, BALANCE, TRANSACTIONS, IDENTITY, INCOME of a customer using their customer Id.
  ```node
    okra_client.getProductsByCustomer(accessToken, {}, (err, results) => {
	    // Handle err
    const product = results;
    });
  ```

* **getBanks**: this returns the list of supported banks
  ```node
    okra_client.getBanks((err, results) => {
	    // Handle err
    const banks = results.banks;
    });
  ```
* **getBankById**: this returns a specific bank info
  ```node
    okra_client.getBankById(bankId,(err, results) => {
	// Handle err
    const bank = results;
    });
  ```
* **getCustomers**: this returns an array of customers associated to your company
  ```node
    okra_client.getCustomers(accessToken, {},(err, results) => {
	    // Handle err
    const customers = results.customers;
    });
  ```
* **mergeIdentities**: Okra offers an api that helps to merge two identical identities into a single identity
  ```node
    okra_client.mergeIdentities(accessToken,
      {
          final:"5e1efaaa848182515cedd066",
          initial:"5e20c13ed2356505c26f5a94",
          options: {}
      },
      (err, results) => {
        // Handle err
      const mergeResult = results;
      });
  ```

Field | Required | Description
---|---|---
**final**<br>`String` | yes | Id of identity to merge into.
**initial**<br>`String` | yes | Id of identity moved from.
**options**<br>`Object` | no | other identities information might want to effect.

**Options Schema**

Key | Description
---|---
**bvn**<br>`Number`| Bank Verification Number of the entity associated with this identity
**nin**<br>`Number`| NIN of the entity associated with this identity
**national_id**<br>`Number`| National Id of the entity associated with this identity
**nims**<br>`String`| NIMs number of the entity associated with this identity
**rc_number**<br>`String`| Company RC number of the entity associated with this identity
**voters_id**<br>`String`| Voters Id of the entity associated with this identity
**marital_status**<br>`String`| Marital Status of the entity associated with this identity
**gender**<br>`String`| Gender of the entity associated with this identity
**dob**<br>`Date`| Date of birth of the entity associated with this identity
**mothers_maiden**<br>`String`| Mother's maiden name of the entity associated with this identity

`accessToken` is a required string to access your account with us. You can find it as client token on the [setting page of the okra dashboard](https://dashboard.okra.ng/settings)

## Data Dictionary

### Auth
Field | Required | Description
---|---|---
**id**<br>`ObjectID` | **Yes** | Unique Auth ID (Unique Okra Identifier)
**validated**<br>`Boolean` | **Yes** | Customer authentication status
**bank**<br>`ObjectID` | **Yes** | Unique Bank ID (Unique Okra Identifier)
**customer**<br>`ObjectID` | **Yes** | Unique Customer ID (Unique Okra Identifier)
**record**<br>`ObjectID` | **Yes** | Unique Record ID (Unique Okra Identifier)
**owner**<br>`ObjectID` | **Yes** | Unique Company ID (Unique Okra Identifier) (Your Client Token)
**created_at**<br>`Object` | **Yes** | Date Auth was fetched
**last_updated**<br>`Object` | **Yes** | Last Date Auth was fetched

### Balance
Field | Required | Description
---|---|---
**id**<br>`ObjectID` | **Yes** | Unique Balance ID (Unique Okra Identifier)
**available_balance**<br>`Integer` | **Yes** | Amount of available funds in account
**ledger_balance**<br>`Integer` | **Yes** | Closing balance of account
**currency**<br>`String` | **Yes** | The currency of the account
**connected**<br>`Boolean` | **Yes** | Customer connection status (Did they choose to connect this account to you)
**env**<br>`String` | **Yes** | Okra API Env the transaction was pulled from **production** or **production-sandbox**
**bank**<br>`ObjectID` | **Yes** | Unique Bank ID (Unique Okra Identifier)
**accounts**<br>`ObjectID` | **Yes** | Unique Account ID (Unique Okra Identifier)
**customer**<br>`ObjectID` | **Yes** | Unique Customer ID (Unique Okra Identifier)
**record**<br>`Array of ObjectID` | **Yes** | Unique Record ID (Unique Okra Identifier)
**created_at**<br>`Object` | **Yes** | Date Balance was fetched
**last_updated**<br>`Object` | **Yes** | Last Date Balance was fetched

### Identity
Field | Required | Description
---|---|---
**id**<br>`ObjectID` | **Yes** | Unique Identifier ID (Unique Okra Identifier)
**firstname**<br>`String` | **Yes** | Customer First Name
**middlename**<br>`String` | **Yes** | Customer Middle Name
**lastname**<br>`String` | **Yes** | Customer Last Name
**next_of_kins**<br>`Identity Object` | **Yes** | Customer Next of Kins
**dob**<br>`Date` | **Yes** | Customer Date of Birth
**verified**<br>`String` | **Yes** | BVN Validation status
**score**<br>`String` | **Yes** | Unique Okra Score
**dti**<br>`String` | **Yes** | Customer Debt to Income Score
**fullname**<br>`String` | **Yes** | Customer Fullname
**company_name**<br>`String` | **Yes | Company Name if Corporate Identity
**nin**<br>`String` | **Yes** | Customer NIN Number
**national_id**<br>`String` | **Yes** | Customer National ID Number
**drivers_lisence**<br>`String` | **Yes** | Customer Driver's License Number
**nimc**<br>`String` | **Yes** | Customer National Identity Management Commission (NIMC) Number
**voters_id**<br>`String` | **Yes** | Customer Voter's ID Number
**rc_number**<br>`String` | **Yes** | Company's Registered Company Number if Corporate Identity
**phone**<br>`Array of String` | **Yes** | Customer Phone Number
**last_login**<br>`String` | **Yes** | Customer Last Login via Okra
**email**<br>`Array of String` | **Yes** | Customer Email address
**address**<br>`Array of String` | **Yes** | Customer
**mothers_maiden**<br>`String` | **Yes** | Customer Mother's Maiden Name
**photo_ids**<br>`Array of Object` | **Yes** | Customer's photo ID
**env**<br>`String` | **Yes** | Okra API Env the transaction was pulled from **production** or **production-sandbox**
**bank**<br>`ObjectID` | **Yes** | Unique Bank ID (Unique Okra Identifier)
**accounts**<br>`ObjectID` | **Yes** | Unique Account ID (Unique Okra Identifier)
**customer**<br>`ObjectID` | **Yes** | Unique Customer ID (Unique Okra Identifier)
**record**<br>`Array of ObjectID` | **Yes** | Unique Record ID (Unique Okra Identifier)
**created_at**<br>`Object` | **Yes** | Date Balance was fetched
**last_updated**<br>`Object` | **Yes** | Last Date Balance was fetched

### Transaction
Field | Required | Description
---|---|---
**id**<br>`ObjectID` | **Yes** | Unique Transaction ID (Unique Okra Identifier)
**debit**<br>`Integer` | **No**| Amount deducted from account 
**credit**<br>`Integer` | **No**| Amount credited to account
**trans_date**<br>`Date` | **Yes** | Date transaction occurred
**cleared_date**<br>`Date` | **Yes** | Date transaction cleared at bank
**unformatted_trans_date**<br>`String` | **Yes** | Date transaction occurred (from bank)
**unformatted_cleared_date**<br>`String` | **Yes** | Date transaction cleared (from bank)
**branch**<br>`String` | **No**| Branch transactions occurred
**ref**<br>`String` | **No**| Bank reference ID (from bank)
**env**<br>`String` | **Yes** | Okra API Env the transaction was pulled from **production** or **production-sandbox**
**code**<br>`String` | **No**| Bank Code (from bank)
**benefactor**<br>`ObjectID` | **No**| Customer ID of sender (within Okra)
**code**<br>`String` | **No**| Bank Code (from bank)
**notes**<br>`Object` | **Yes** | Breakdown of Narrative from bank
**bank**<br>`ObjectID` | **Yes** | Unique Bank ID (Unique Okra Identifier)
**account**<br>`ObjectID` | **Yes** | Unique Account ID (Unique Okra Identifier)
**record**<br>`Array of ObjectID` | **Yes** | Unique Record ID (Unique Okra Identifier)
**created_at**<br>`Object` | **Yes** | Date transactions was fetched
**last_updated**<br>`Object` | **Yes** | Last Date transactions was fetched

### Notes
Field | Required | Description
---|---|---
**desc**<br>`String` | **Yes** | Narrative / Description of transaction (combination of bank and user entered information)
**topics**<br>`Array of String` | **Yes** | Topics within the desc
**places**<br>`Array of String` | **Yes** | Places mentioned within the desc
**people**<br>`Array of String` | **Yes** | People mentioned within the desc
**actions**<br>`Array of String` | **Yes** | Actions mentioned within the desc
**subject**<br>`Array of String` | **Yes** | Subject of the desc
**preposition**<br>`Array of String` | **Yes** | Prepositions within desc to understand intent

> For more information checkout [okra's documentation](https://docs.okra.ng)

## Contributing

Please feel free to fork this package and contribute by submitting a pull request to enhance the functionalities.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
