# Sample Service Catalog Items

A collection of easy-to-digest sample Service Catalog items that can be used as a baseline for your service catalog. [LIST OF ITEMS FEATURED AS TO WHY THIS IS HELPFUL]

## Table of Contents

-   [Install a Catalog with an Unlocked Package](#install-a-catalog-with-an-unlocked-package-recommended): **RECOMMENDED**. This option lets you customize a catalog without requiring you to install a local development environment. Ideal for admins with zero development knowledge.

-   [Deploy the Sample Catalog Items to an Org](#deploy-the-sample-catalog-items-to-an-org): This option lets you use sample catalog items as a baseline for advanced building in your own catalog. Ideal for developers and advanced admins.

## Install a Catalog with an Unlocked Package (RECOMMENDED)

These instructions deploy the catalog to a sandbox. (WHY WOULD WE DO THIS?) We suggest that you avoid installing this package in production and instead use it as a baseline for building your own catalog. (to the explore the features of service catalog?) (QUESTION: And is this sensible for admins with zero dev knowledge to build a catalog on their own? Because this sounds like what we're already saying in Deploy the Sample Catalog Items in an Org.)

**Prerequisite:** Assign the necessary permissions to your users by following the steps in [this documentation](link to be added).

1. Log in to your org.

1. Install the [Sample Service Catalog Items package](https://login.salesforce.com/packaging/installPackage.apexp?p0=04t4W0000038VvfQAE) into your org.

1. Select **Install for All Users**.

1. From Setup, in the Quick Find box, enter `Service Catalog` and click **Get Started**.

1. Choose the samples most similar to the items you want to create in your catalog.

## Deploy the Sample Catalog Items to an Org

This option is for developers or advanced admins who want to use these sample catalog items as a baseline for advanced building. (NOTE: THIS IS JUST A REHASH OF THE SUMMARY FOR THIS SECTION. IT DOESN'T MATCH THE INTRO OF THE PREVIOUS SECTION EITHER.)

1. From the [GitHub Salesforce Deploy Tool](https://githubsfdeploy.herokuapp.com?owner=CovidBackToWork&repo=service-catalog-recipes), click **Login to Salesforce**.

1. If asked, click **Allow** to let the **Deploy to Salesforce** tool perform the requested actions.

1. Enter your org credentials.

1. When you're redirected back to the Salesforce page, click **Deploy**.

## (Draft) Scratch Org (NOT IN TABLE OF CONTENTS - PLEASE DISCUSS)

1. If you haven't done so yet, authorize your hub org and provide it with an alias. (We use `myhuborg` in this example.)

    ```
    sfdx auth:web:login -d -a myhuborg
    ```

1. Clone the service catalog recipes repository.

    ```
    git clone https://github.com/CovidBackToWork/service-catalog-recipes
    cd service-catalog-recipes
    ```

1. Create a scratch org and provide it with an alias. (We use `service-catalog-recipes` in this example.)

    ```
    sfdx force:org:create -s -f config/project-scratch-def.json -a service-catalog-recipes
    ```

1. Assign the `ServiceCatalogBuilder` permission set to the scratch org user.

    ```
    sfdx force:user:permset:assign -n ServiceCatalogBuilder
    ```

1. Assign the `EmployeeExperience` permission set license to the scratch org user.

    ```
    echo "insert new PermissionSetLicenseAssign(AssigneeId = UserInfo.getUserId(), PermissionSetLicenseId = [SELECT Id FROM PermissionSetLicense WHERE DeveloperName='EmployeeExperiencePsl']?.Id);" | sfdx force:apex:execute
    ```

1. Create a permission set to access Employee objects.

    ```
    sfdx force:source:deploy -p force-app/unpackaged/permissionsets
    ```

1. Assign the `EmployeeObjectsAccess` permission set to the scratch org user.

    ```
    sfdx force:user:permset:assign -n EmployeeObjectsAccess
    ```

1. Push the app to your scratch org.

    ```
    sfdx force:source:push -f
    ```

1. Assign the `ServiceCatalogRecipesAccess` permission set for the dev recipes use cases to the scratch org user.

    ```
    sfdx force:user:permset:assign -n ServiceCatalogRecipesAccess
    ```

1. Open the scratch org.

    ```
    sfdx force:org:open
    ```
