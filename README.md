suds-marketo
============

suds-marketo is a python query client that wraps the Marketo SOAP API. This package is based on https://github.com/segmentio/marketo-python but uses SUDS instead of manual XML requests. Using SUDS makes it easier to update and allows access to unimplemented functions by calling the suds methods directly (client.yourFunction()).

Marketo SOAP Api: https://jira.talendforge.org/secure/attachmentzip/unzip/167201/49761%5B1%5D/Marketo%20Enterprise%20API%202%200.pdf

## Get Started

```
pip install suds-marketo
```

```python
import suds_marketo
client = suds_marketo.Client(soap_endpoint='https://na-q.marketo.com/soap/mktows/2_0',
                        user_id='bigcorp1_461839624B16E06BA2D663',
                        encryption_key='899756834129871744AAEE88DDCC77CDEEDEC1AAAD66')
```

## See available functions and objects

You can see the list of functions and objects available by printing the suds client object.

```python
> print client.suds_types
[ActivityRecord, ActivityType, ActivityTypeFilter, ArrayOfActivityRecord, ArrayOfActivityType, ArrayOfAttrib, ..., SuccessSyncMultipleLeads, SyncCustomObjStatus, SyncOperationEnum, SyncStatus, SyncStatusEnum, VersionedItem]
> print client.suds_methods
[getCampaignsForSource, deleteCustomObjects, syncMultipleLeads, deleteMObjects, describeMObject, listOperation, mergeLeads, getCustomObjects, getLead, getImportToListStatus, importToList, syncLead, getMObjects, getLeadActivity, getLeadChanges, syncMObjects, scheduleCampaign, listMObjects, syncCustomObjects, requestCampaign, getMultipleLeads]

```

You can access the methods as follow:
```python
> client.getLead(lead_key)
```
You can access the types as follow:
```python
> client.LeadKey # Simple type
> client.LeadKeyRef.EMAIL # Enumeration
```

## Call functions

If the function is defined in the client class:
```python
> lead = client.get_lead('user@gmail.com')
```

If the function you are looking for is not defined in the client class:

> lead_key = client.LeadKey # You need to create the proper object to pass to the function
> lead_key.keyType = client.LeadKeyRef.EMAIL
> lead_key.keyValue = email
> client.set_header() # You need to sign the header every time you make a call to the SOAP Api
> resp = client.getLead(lead_key)

### Error

An Exception is raised if the lead is not found, or if a Marketo error occurs.

```python
from suds import WebFault
try:
    lead = client.get_lead('test@punchtab.com')
except WebFault as e:
    print e
```
As described in the Appendix B of the Marketo API, you can access the following error attributes:
```
e.fault.faultcode
e.fault.faultstring
e.fault.detail
e.fault.detail.serviceException
e.fault.detail.serviceException.name
e.fault.detail.serviceException.message
e.fault.detail.serviceException.code
```
