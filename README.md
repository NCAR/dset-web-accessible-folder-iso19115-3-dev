# dset-web-accessible-folder-iso19115-3
Repository for "new ISO" records

Using an NCAR-based automated service, newly created and updated ISO XML records in this GitHub repository are automatically converted to "old ISO" and pushed to a sibling WAF for harvesting by CKAN.

## Performing Create, Update, Delete Operations for Records

Here we provide a few notes on how to perform successful Create, Update, and Delete operations on records in this WAF.

### Create

A newly created record will be translated successfully if the following conditions are met:

* The file ends in `.xml`
* The identifier used in `<mdb:metadataIdentifier>` is unique, that is, not already present in a different record harvested by CKAN.
* The file contents meet the requirements of the ISO 19115-3 schema.

It also helps if the first 50 characters in the record's `<CI_Citation>` title are unique among the records in this WAF, as a unique Landing Page URL will be created for this record in CKAN.

### Update

A newly updated record will be translated and updated in CKAN successfully if the following conditions are met:

* All the above conditions for the "Create" operation are met.
* The Date/time in `<mdb:dateInfo>` is set to a value further ahead in time than its previous value.
* The Date/time in `<mdb:dateInfo>` is set to a value that is not more than 24 hours ahead of today's date.

The last two conditions will be tested by the CKAN harvester, rather than by the translation service.   In other words, the translation will still occur and the translated record will be pushed to CKAN, but the harvester will either:
* not harvest the record if the `<mdb:dateInfo>` is unchanged or set to a value further in the past than its previous value.
* raise a validation error if the new date is more than 24 hours ahead of today's date.

### Delete

One way of deleting a record from CKAN is to rename the file so that it no longer ends in `.xml`.   For example, the file `record.xml` could be renamed to `record.xml.IGNORE`.   In this case, the translation service will still translate the record, but the translated record's filename change will cause CKAN to delete the file and not harvest it until the file name is changed again.

Another way to delete the record from CKAN is, of course, to simply delete the file from this WAF. 


## Special Cases

The translator service will currently attempt to translate every file in the WAF, including this README.md file.   Translating a non-XML file results in a translation error.   In this case, for this or any file with a translation error of any kind, no resulting file is pushed to the target "old ISO" WAF.
