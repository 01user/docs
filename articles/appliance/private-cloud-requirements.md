# Information Requirements for Setting Up the Appliance in Auth0's Private Cloud

If you have decided to purchase an Appliance that is hosted in a dedicated area of Auth0's cloud, Auth0 will set up the Appliance on your behalf. To do so, Auth0 asks that you provide the following information:

* **Preferred AWS region** such as AWS US-West-2, AWS US-East-1, AWS EU-Central-1, etc;
* The **email address for at least one administrator**. This administrator will be able to add additional administrators as necessary;
* **Six (6) DNS names**:
    * Three (3) will be used for the non-Production node, and three (3) are for the Production cluster.
    * DNS names **must** be finalized for all tenants prior to Appliance deployment. They cannot be changed afterwards.

## Further Reading

* [DNS](/appliance/infrastructure/dns)
* [DNS Records](/appliance/infrastructure/network#dns-records)
