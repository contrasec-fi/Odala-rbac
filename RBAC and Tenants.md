# 1. RBAC and Tenants

Tenancy is logical data separation. Some brokers, like Scorpio and Orion support multitenancy. This together with RBAC allows more detailed data access granularity.

# 2. Example of Tenant configuration in Umbrella

1. Create a required role in "Global request settings" which does not exist. This will force Umbrella to evaluate Sub-URL rules.

![](/pictures/ten1.JPG)

2. Create a wanted sub URL rule:

![](/pictures/ten2.JPG)

# 2. References

[Umbrella Read the docs](https://api-umbrella.readthedocs.io/en/latest/)

[Orion Read the docs](https://fiware-orion.readthedocs.io/en/master/user/multitenancy/index.html#multi-tenancy)
