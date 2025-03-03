<!-- loio508d406ac92043dba95f694144803c26 -->

# Business Configuration Maintenance Object Cloud Platform API

Use the ABAP API *mbc\_cp\_api* to create, update, delete, and read business configuration maintenance objects.



After a business configuration maintenance object has been created, it will be shown on the list of all maintainable business configurations in the Fiori app *Custom Business Configurations* if you have all necessary authorizations regarding the service of the business configuration. You can get the necessary authorizations by following the instructions in [Business Configuration Maintenance Object](business-configuration-maintenance-object-61159c4.md) \> **Provide Authorizations for a Business Configuration**. The end user can then maintain data for the registered business configuration from the frontend.

Before using any of the following methods, first obtain an interface handle by calling `mbc_cp_api=>business_configuration_api` with an identifier for your business configuration.

If an error occurs while calling the following methods, an exception of type `cx_mbc_api_exception` will be raised. Use the method `if_xco_news~get_messages` to retrieve all messages that the exception carries.



### Create a Business Configuration Maintenance Object

Use the `create` method of the obtained business configuration interface handle to create a new object. Supply the parameters as illustrated in the code sample below.

-   For additional information about the parameters, see [here](https://help.sap.com/docs/BTP/5371047f1273405bb46725a417f95433/8ea18fd0f93f4cd48578d2c75f3c8c89.html?version=Cloud).

-   A transport request of type `Workbench` to write the business configuration registration to. An entry of type "SMBC" \(Business Configuration Object\) will be written to that transport request.


> ### Sample Code:  
> ```abap
> CLASS zcl_ys_register_bc DEFINITION PUBLIC FINAL CREATE PUBLIC
>     INHERITING FROM cl_xco_cp_adt_simple_classrun.
>   PROTECTED SECTION.
>     METHODS:
>       main REDEFINITION.
> ENDCLASS.
> 
> CLASS zcl_ys_register_bc IMPLEMENTATION.
>   METHOD main.
>     DATA(lo_business_configuration) = mbc_cp_api=>business_configuration_api('/Y123456/HOLIDAY_CALENDAR').
> 
>     TRY.
>         lo_business_configuration->create(
>           iv_name            = 'Holiday Calendar'
>           iv_description     = 'A calendar for holidays'
>           iv_service_binding = '/Y123456/CAL_I_HOLIDAY_SB'
>           iv_service_name    = 'Holiday'
>           iv_service_version = 0001
>           iv_root_entity_set = 'HolidayRoot'
>           iv_transport       = 'X02K900025'
>         ).
>       CATCH cx_mbc_api_exception INTO DATA(lx_mbc_api_exception).
>         DATA(lt_messages) = lx_mbc_api_exception->if_xco_news~get_messages( ).
> 
>         LOOP AT lt_messages INTO DATA(lo_message).
>           " Use lo_message->get_text( ) to get the error message.
>         ENDLOOP.
>     ENDTRY.
>   ENDMETHOD.
> ENDCLASS.
> 
> ```



### Update a Business Configuration Maintenance Object

You can only update the name and the description of your business configuration by using the respective methods `update_name` and `update_decription`. A transport request of type Workbench must be supplied to transport the updated object.



### Delete a Business Configuration Maintenance Object

To delete a business configuration maintenance object, use the method `delete`. A transport request of type `Workbench` must be supplied to transport the deletion.



### Check if a Business Configuration Maintenance Object Exists

To check the existence of a business configuration maintenance object, use the method `exists`. The method has no parameters. It returns `abap_true` if the object exists, `abap_false` if it doesn't.

For an overview of business configuration maintenance objects, use CDS view `I_CustABAPObjDirectoryEntry` with `ABAPObjectType = 'SMBC'`.



### Read a Business Configuration Maintenance Object

To retrieve the details of a business configuration maintenance object, use the method `read`. The method has no parameters. The returned structure contains all the metadata of the business configuration maintenance object.

