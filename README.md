# Keycloak Custom Mapper Example

## Overview

This project provides a custom protocol mapper for Keycloak, allowing you to extend Keycloak's functionality by implementing a custom protocol mapper for specific authentication protocols.

The use case for this protocol mapper is to map a specific claim, based on a user attribute and do some logic with the attribute value.

This was tested with Keycloak 23.0.1.

## Installation

To use this custom protocol mapper in your Keycloak instance, follow these steps:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/keycloak-custom-protocol-mapper.git
   ```

2. **Compile the project**
   ```bash
   mvn clean install
   ```
3. **Copy the artifact to providers directory on Keycloak**

4. **Build keycloak to use the provider**
   ```bash
   ./kc.sh build
   ```

## Use case

My users has an attribute named **my-custom-attribute**. This attribute follows this pattern **##KEY:VALUE####KEY:VALUE##**.

My logic here is, if the the user has the KEY:VALUE equals to **##ABC:DEF##**, i fill the claim **groupAttribute** with this value, otherwise, the claim **groupAttribute** is empty.

See the example below of an access token generated where the user has the value **##123:456####ABC:DEF##** on **my-custom-attribute**:

```json
{
  "exp": 1701739507,
  "iat": 1701739207,
  "jti": "22905e0f-cb04-4c2b-9593-6890319ceabb",
  "iss": "http://localhost:8080/realms/test-realm",
  "aud": "account",
  "sub": "824395ce-13ef-41c4-8612-a423df5e21f0",
  "typ": "Bearer",
  "azp": "test-client",
  "session_state": "49711db6-a6bb-48e0-8e1c-c93273504d0b",
  "acr": "1",
  "realm_access": {
    "roles": [
      "default-roles-test-realm",
      "offline_access",
      "uma_authorization"
    ]
  },
  "resource_access": {
    "account": {
      "roles": [
        "manage-account",
        "manage-account-links",
        "view-profile"
      ]
    }
  },
  "scope": "openid email profile",
  "sid": "49711db6-a6bb-48e0-8e1c-c93273504d0b",
  "groupAttribute": "ABC:DEF",
  "email_verified": false,
  "name": "Gabriel Padilha",
  "preferred_username": "gabsanto",
  "given_name": "Gabriel",
  "family_name": "Padilha"
}
```
