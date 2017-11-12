# Data model


##Process overview


Multiple events imported through API or mediation (mass file processing), are transformed into EDR or usage events, that are attached to the subscription.



*Data model*
![data-model](data-model.png "Data model")

## Provider and seller 

*Provider, sellers and brands*
![provide-seller](provider-seller.png "Provider,seller and brands")


## Customer structure

The customer is the entity holding a contract with the provider and thus being billed by it.
![customer-structure](customer-structure.png "Customer structure")


* **Billing accounts** contain all the invoices from the user account subscriptions below and particularly the language parameter, meaning the same customer account can be billed with multiple language configurations.
* **User accounts** carry subscriptions and wallet operations (a default wallet, called principal wallet, is used for postpaid transactions and a secondary wallet is available for prepaid transactions).
* **Subscriptions** are attached to the user account. They carry instances of catalog services and charges (through the bundling offer) that are used in the rating engine to produce rated wallet operations.
* **Access points** are the identifier allowing usage events to be attached to the subscription.
![customer-hierarchy](customer-hierarchy.png "Customer structure")

## Billing catalog structure

Catalogue configuration is the key concept required to design the pricing policy of the provider. The catalog structure and catalog price hierarchy is organized as follows:
![catalog-structure](catalog-structure.png "Catalog structure")

In the Opencell data model, catalog structure and catalog prices are two distinct concepts.

### Wallets, charges, services & offers 






* Service termination occurs when an active service instance is terminated. Depending on the cause of the termination, the termination charges can be instantiated and automatically rated.
* Other types of one-shot charges exist independently of any service. They can be applied to an existing subscription and rated automatically as in case, for example, of a penalty fee for late payment.

* Service activation occurs when a service with recurring charges is activated. All recurring charges are instantiated and automatically rated for the first calendar period, potentially with a prorata calculation. 
* Service termination occurs when an active service instance is terminated. Depending on the cause of the termination, recurring charges can be rated until the end of the contract or just once with a prorata calculation.
* Recurring charges can be prorated at subscription or termination and can be charged at the beginning of the cycle or at the end.









*Price plan configuration*
![price-plan-configuration](price-plan-configuration.png "Price plan configuration")

# Security, access and SSO

## Users, roles and permissions

By default, users have roles and permissions stored in database, and permissions are used throughout the services and UI layers to authorize access.

A permission entity consists in a permission action (read, update, delete…) and a resource (invoice, account…).
The association between users and permission is not done directly at the user level but through a list of roles that are themselves a list of permissions. 

A user is characterized by a unique username (unique among all providers), a password, a list of roles and a list of providers. When logged in, a user associated with several providers will be prompted to choose one provider in his web session.

## Identity and access management

Opencell uses Keycloak, an open source identity and access management solution that is part of Wildly, to provide SSO and identity brokering. 

Opencell uses Keycloak to provide:
* Single-sign-on 
* Identity brokering and social login
* User federation
* Standard protocols (Oauth2, SAML, OpenID)
* Authorization services
* Multi-LDAP integration

# Providers and multi-tenancy

Opencell is multi-tenant physically. Each tenant running a schema and instance in a JBOSS or on another image (VM, Docker…) is physically separated from all other tenants present on the Opencell instance. 

Providers are entities selling services to their customers, potentially through a network of resellers. Since all entities in Opencell, from catalog to accounts, are associated to a unique provider, the same JBOSS instance can host multiple Opencell instance with their providers: their data will be completely separated from the data of the other providers.

# API management and integration

## API


* Accounts
* Quotes
* Orders
* Workflows
* Rating
* Invoicing
* Settings
* Mediation
* Dunning
* Payment

* Product Order
* Product Ordering

## Other integration tools

Opencell provides additional tools such as :

* Webhooks
* XML import
* Notifications


## Development language

### Back-end developemnt

Back-end is fully developped in Java EE 8 and runs on Wildfly middleware including Infinispan, ActivMQ and Keycloak.

### Front-end development

Front-end is developed using JavaScript React. This currently covers the customer care and selfcare modules but all GUI is expected to be developed using React. 


## Third party software

Like many open source software companies, Opencell relies significantly on proven third-party components.  

These include technologies such as:

* Java Enterprise Edition 8 and JavaServer Faces (JSF)
* Primefaces (Apache 2.0)
* Joda Time (Apache 2.0)
* Jackson 2.1.4 (Apache 2.0)
* Xalan 2.7 (Apache 2.0)
* Jasper Reports library (LGPL)

