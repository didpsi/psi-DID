# Police Science Institution DID Method Specification

This is version 0.0.1 of the psi DID Method Specification.

</br>

## Abstract

The Police Science Institution is the only public security research institute in Korea that conducts convergence research between social science and science technology. In particular, with the establishment of the Science and Technology Research Department in 2015, it has established itself as the only public safety and security research institute in Korea that integrates public safety science and technology fields.

In addition, we are promoting R&D to provide high-quality public security services to the Korean people and to strengthen the security capabilities of on-site police officers through research on public safety science. Using DID we are expecting to protect user privacy from social science or social crime. With DID Documents, user can authenticate,encrypt,issue,verify data and base on this operation user can achieve confidentiality/integrity.

</br></br>

## Status Of this document

This is a draft document and will be updated based on W3C.

</br></br>

## Police Science Institute DID Method Identifier

The did identifier of Police Science Institute is `psi`. The did method format of the did:psi identification system is composed of the following format.

```bash
# psi-did
"did:psi" + psi-identifier

# psi-identifier
21, 22 (base58char)

# base58char 
"1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9" / "A" / "B" / "C" / 
"D" / "E" / "F" / "G" / "H" / "J" / "K" / "L" / "M" / "N" / "P" / "Q" / 
"R" / "S" / "T" / "U" / "V" / "W" / "X" / "Y" / "Z" / "a" / "b" / "c" / 
"d" / "e" / "f" / "g" / "h" / "i" / "j" / "k" / "m" / "n" / "o" / "p" / 
"q" / "r" / "s" / "t" / "u" / "v" / "w" / "x" / "y" / "z"
```

Example did of Police Science Institute did is `did:psi:J9w22Ajvoo41Yg2NZd93NE`. Example did document of `did:psi:J9w22Ajvoo41Yg2NZd93NE` is

```json
{
    "@context": "https://w3id.org/did/v1",
    "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
    "controller": "did:psi:9waFSPxAbjLQJ9rFhtYeQS",
    "authentication": ["did:psi:J9w22Ajvoo41Yg2NZd93NE#0"],
    "assertionMethod": ["did:psi:J9w22Ajvoo41Yg2NZd93NE#1"],
    "keyAgreement": ["did:psi:J9w22Ajvoo41Yg2NZd93NE#2"],
    "verificationMethod": [
        {
            "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE#0",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "controller": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
            "publicKeyBase58": "zbPJfARDmbshQ2iSZ3fg5WxAh9VEXAoiyi6QRBJBBj2z"
        },
        {
            "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE#1",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "controller": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
            "publicKeyBase58": "24YpP4L2ydSffMn92KF1EvzgwcStdzixut1CDizuCpaqD"
        },
        {
            "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE#2",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "controller": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
            "publicKeyBase58": "277hEJxdh596ptwNmyApNTa1TZLMjgpEeQgXbTbXGYns1"
        }
    ]
}
```

</br>

All did:psi identifiers are base58 encoded with 16 bytes of uuid alphabet. Purpose of using base58 is To avoid readability issues for 0, O, I, l characters. A convenient regex to match did:psi identifier is:

```bash
^(did:psi:(?:[1-9A-HJ-NP-Za-km-z]{21,22}))$
```

</br></br>

# Operation

`did:psi` operations(CRUD) are provided directly by did contract. DID Documents are created in blockchain and stored in did table. To operate DID Documents, you should first execute prerequest which is creating an account of the blockchain.

We convert data types to efficiently use the resources of the blockchain. UUID which is commonly string is converted into a BigInt and stored in the blockchain. Note that all types referred to as UUID in the subsection are converted from BigInt type.

</br>

## Prerequest

Prerequest step is creating an account. We created an newaccount action which is related to user's account, did controller. You need an 1. public key for an account, 2. creator account which will pay resources, 3. new acocunt name and an 4. random uuid for controller. It will return an transaction id if successfully created.

```bash
# endpoint
did contract/newaccount

# Input
{PUBLIC_KEY_FOR_ACCOUNT, CREATOR_ACCOUNT, USER_ACCOUNT, RANDOM_UUID}

# Output
{transaction Id}
```

</br>

Example input data

```bash
# endpoint
did contract/newaccount

# Input
{"EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV", "creator", "user", 96243185242488690427955470100060324235}

# Output
{transaction Id}
```

</br>

---

## Create

To create an `did:psi` identifier(DID Document), you should use regdid action of did contract. Executing the regdid action will store the input data to the blockchain did table. To use blockchain resources efficiently, the format of did document is different as W3C. Instead for compatibility with other did specification, DID Document read operation is converted to W3C format.

```bash
# endpoint
did contract/regdid

# Input
{
    CONTROLLER_ACCOUNT, UUID, VERIFICATION_METHOD, AUTHENTICATION, ASSERTION_METHOD, KEY_AGREEMENT
}

# Output
{transaction Id}
```

</br>

Example 

```bash
# endpoint
did contract/regdid

# Input 
{
  controller: 96243185242488690427955470100060324235,
  uuid: 184651560173348009785121532896741846359,
  verificationMethod: [
    ...
    {
      publicKey: 'PUB_K1_7bhWFK2tqeNY2PNbKUE7nSjzeYerNuvLyPMr3Qo1bHKWx2az3a',
      controller: '184651560173348009785121532896741846359'
    },
    ...
  ],
  authentication: [ { uuid: '184651560173348009785121532896741846359', index: 0 } ],
  assertionMethod: [ { uuid: '184651560173348009785121532896741846359', index: 1 } ],
  keyAgreement: [ { uuid: '184651560173348009785121532896741846359', index: 2 } ]      
}

# Output
{transaction Id}
```

</br>

---

## Read

To read did document from a specific did, you can use did:psi resolver. The input value and output value are following.

```bash
#endpoint
did:psi resolver endpoint

# Input
{did}

# Output
{DID Document}
```

</br>

Example

```bash
#endpoint
did:psi resolver endpoint

# Input
{"did:psi:J9w22Ajvoo41Yg2NZd93NE"}

# Output
{
    "@context": "https://w3id.org/did/v1",
    "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
    "controller": "did:psi:9waFSPxAbjLQJ9rFhtYeQS",
    "authentication": ["did:psi:J9w22Ajvoo41Yg2NZd93NE#0"],
    "assertionMethod": ["did:psi:J9w22Ajvoo41Yg2NZd93NE#1"],
    "keyAgreement": ["did:psi:J9w22Ajvoo41Yg2NZd93NE#2"],
    "verificationMethod": [
        {
            "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE#0",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "controller": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
            "publicKeyBase58": "zbPJfARDmbshQ2iSZ3fg5WxAh9VEXAoiyi6QRBJBBj2z"
        },
        {
            "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE#1",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "controller": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
            "publicKeyBase58": "24YpP4L2ydSffMn92KF1EvzgwcStdzixut1CDizuCpaqD"
        },
        {
            "id": "did:psi:J9w22Ajvoo41Yg2NZd93NE#2",
            "type": "EcdsaSecp256k1VerificationKey2019",
            "controller": "did:psi:J9w22Ajvoo41Yg2NZd93NE",
            "publicKeyBase58": "277hEJxdh596ptwNmyApNTa1TZLMjgpEeQgXbTbXGYns1"
        }
    ]
}
```

</br>

---

## Update verificationMethod

To update verificationMethod you should execute updatekeys action. Updatekeys action input is a list of verifiaction method objects.

```bash
# verificationMethod object
{INDEX, PUBLICKEY, CONTROLLER}
```

</br>

Following input value is used for executing updatekeys action.

```bash
#endpoint
did contract/updatekeys

# Input
{CONTROLLER_UUID, DID_UUID, [VERIFICATION_METHOD_OBJECT]}

# Output
{transaction ID}
```

</br>

Example input data

```bash
#endpoint
did contract/updatekeys

# Input 
{
    "controller": 96243185242488690427955470100060324235,
    "uuid": 184651560173348009785121532896741846359,
    "verificationMethod": [
      {
        "index": 10,
        "publicKey": "PUB_K1_7VsJLbbTJtvTfjTAJ9WLa2wTn4P7fBk71UckBDtFHkerou1hDd",
        "controller": 184651560173348009785121532896741846359
      },
      {
        "index": 11,
        "publicKey": "PUB_K1_7wkd9kjfdMbxRYCrvYUbJdCKWYn8hqQxBMchLjQCgTpdwc4PFr",
        "controller": 184651560173348009785121532896741846359
      },
      {
        "index": 12,
        "publicKey": "PUB_K1_8EYpZU9EL1meLKErcV9RJzmS5gHN4Tzn9rtQtHuyqzvhFeLAXc",
        "controller": 184651560173348009785121532896741846359
      }
    ]
}

# Output
{transaction ID}
```

</br>

---

## Update verificationRelationship

To update verificationRelationship you should execute the proper action which are listed below. To update authentication use addauth/rmauth. To update assertionMethod use addasserter/rmasserter. To update keyAgreement use addkeyagrm/rmkeyagrm. You can only update verificationRelationship when the value you want to update exist in verificationMethod.

```bash
# update authentication
addauth, rmauth

# update assertionMethod
addasserter, rmasserter 

#  update keyAgreement
addkeyagrm, rmkeyagrm
```

These structure of the input parameter is all the same. Following structure is used for updating verificaionRelationship.

```bash
# verificationRelationship object
{UUID, INDEX}
```

</br>

Following input value is used for updating verificationRelationship.

```bash
#endpoint
did contract/ACTION_YOU_WANT_TO_UPDATE

# Input 
{CONTROLLER_UUID, DID_UUID, [VERIFICATION_RELATIONSHIP_OBJECT]}

# Output
{transaction ID}
```

</br>

Example input data

```bash
#endpoint
did contract/ACTION_YOU_WANT_TO_UPDATE

# Input 
{
    "controller": 96243185242488690427955470100060324235,
    "uuid": 184651560173348009785121532896741846359,
    "authenticator": { "uuid": 184651560173348009785121532896741846359, "index": 10 }
}

# Output
{transaction ID}
```

</br>

---

## Delete(Deactivate)

To delete DID Document that was registered, you should use deletedid action of did contract. Executing the deletedid action will remove the input did from blockchain did table. If the transaction successfully executed, it will return transaction ID.

```bash
#endpoint
did contract/deletedid

# Input 
{
    CONTROLLER_UUID, DID_UUID
}

# Output
{transaction Id}
```

</br></br>

Example input data

```bash
endpoint : did contract/deletedid

# Input 
{
    "controller": 96243185242488690427955470100060324235,
    "uuid": 184651560173348009785121532896741846359
}

# Output
{transaction Id}
```

</br></br>

# Security Considerations

The did:psi identification system is designed to acheive full SSI(Self Sovereign Identity). Which means only the owner of the data could have control. We decided to create a new permission in blockchain and when the user proves it has control(which will be signing), then the user could do CRUD operations we implemented. It is designed so that only user accounts with the appropriate permision can modify DID Documents.

</br></br>

# Privacy Considerations

Police Science Institute is considering user information without PII(Personally-Identifiable Information). In using Police Science Institute did, there is no private information uploaded in blockchain. Only public key information is registered. Using did document properties we issue/verify verifible credentials and verifiable presentation. And also when sharing data based on  Police Science Institute did, we provide end-to-end encryption. 


</br></br>

## References

[1] Decentralized Identifiers (DIDs) v1.0, [W3C did-core](https://www.w3.org/TR/did-core/)

[2] Police Science Institute (PSI), Korea, [Police Science Institute](www.psi.go.kr)