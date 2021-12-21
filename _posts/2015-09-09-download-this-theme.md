---
title: Publishing B2C Store in Hybris 5.6
updated: 2015-10-09 10:38
---

##Synopsis
This blog post summarizes the steps involved in publishing an OOB B2C electronic accelerator (starter store) on hybris 5602 commerce suite, It
further describes the steps to publish a customized version of the store based on the OOB B2C Accelerator code template.
<div class="divider"></div>

##Setup and Build
Download Hybris Commerce Suite 5602 from Hybris WIKI [https://download.hybris.com/resources/releases/5.6.0/hybris-commerce-suite-5.6.0.2.zip](https://download.hybris.com/resources/releases/5.6.0/hybris-commerce-suite-5.6.0.2.zip)
Unzip the file to your preferred local folder and execute following from command prompt.

```bash
cd ~/hybris/bin/platform
. ./setantenv.sh
ant clean all
....

[input]  Press [Enter] to use the default value ([develop], production)
develop
...
...


all:
     [echo] Build finished on 3-October-2015 09:34:54.
     [echo]        

BUILD SUCCESSFUL
Total time: 43 seconds
```

At the end of this step you have a hybris commerce suit built using development template.

##Setting up localextensions.xml

Unlike previous versions, localextensions.xml generated with development template is empty by default, you need to add the extensions that are relevant for your project.
Here is a sample localextensions.xml for publishing OOB B2C electronic store

``` xml
<?xml version="1.0" encoding="UTF-8"?>  
<hybrisconfig xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="resources/schemas/extensions.xsd">  
    <extensions>  
        <!--  
                All extensions located in ${HYBRIS_BIN_DIR}/platform/ext are automatically loaded.  
                More information about how to configure available extensions can be found here : https://wiki.hybris.com/x/nZVzC  
        -->  
                   
        <path dir="${HYBRIS_BIN_DIR}" />  
                                 
        <!-- ext-platform -->  
        <extension name="admincockpit" />  
        <extension name="backoffice" />  
        <extension name="cockpit" />  
        <extension name="hmc" />  
        <extension name="mcc" />  
        <extension name="platformhmc" />  
                                           
        <!-- ext-platform-optional -->  
        <extension name="ldap" />  
        <extension name="lucenesearch" />  
        <extension name="virtualjdbc" />  
                                           
        <!-- ext-cockpit -->  
        <extension name="reportcockpit" />  
                                           
        <!-- ext-commerce -->  
        <extension name="btg" />  
        <extension name="btgcockpit" />  
        <extension name="commercesearch" />  
        <extension name="commercesearchbackoffice" />  
        <extension name="commercesearchhmc" />  
        <extension name="commerceservices" />  
        <extension name="basecommerce" />  
        <extension name="payment" />  
        <extension name="promotions" />  
        <extension name="voucher" />  
        <extension name="customerreview" />  
        <extension name="ticketsystem" />  
        <extension name="solrfacetsearch" />  
        <extension name="solrfacetsearchhmc" />  
        <extension name="oci" />  
        <extension name="wishlist" />  
        <extension name="commercefacades" />  
        <extension name="commercewebservicescommons" />  
                                      
        <!-- ext-content -->  
        <extension name="productcockpit" />  
        <extension name="cms2" />  
        <extension name="cms2lib" />  
        <extension name="cmscockpit" />  
        <extension name="bmecat" />  
        <extension name="bmecathmc" />  
        <extension name="importcockpit" />  
        <extension name="classificationsystems" />  
        <extension name="classificationsystemshmc" />  
                                    
        <!-- ext-channel -->  
        <extension name="cscockpit" />  
        <extension name="mobileoptionals" />  
        <extension name="mobileservices" />  
                                   
        <!-- ext-addon -->  
        <extension name="addonsupport" />  
        <extension name="b2ccheckoutaddon" />  
  
        <!-- ext-print -->  
        <extension name="print" />  
        <extension name="printcockpit" />  
        <extension name="printhmc" />  
  
        <!-- ext-template -->  
        <extension name="ycommercewebservices" />  
        <extension name="ycommercewebserviceshmc" />  
                                        
        <!-- ext-accelerator -->  
        <extension name="acceleratorservices" />  
        <extension name="acceleratorfacades" />  
        <extension name="acceleratorcms" />  
        <extension name="acceleratorstorefrontcommons" />  
          
          
        <extension name="yacceleratorinitialdata" />  
    <extension name="yacceleratorcockpits" />  
    <extension name="yacceleratorfulfilmentprocess" />  
    <extension name="yacceleratorcore" />  
    <extension name="yacceleratorfacades" />  
    <extension name="yacceleratorstorefront" />  
    <extension name="electronicsstore" />  
    <extension name="apparelstore" />  
    <extension name="yacceleratortest" />  
     
    <extension dir="${HYBRIS_BIN_DIR}/ext-addon/b2ccheckoutaddon"/>  
    <extension dir="${HYBRIS_BIN_DIR}/ext-addon/addonsupport"/>  
    <extension dir="${HYBRIS_BIN_DIR}/ext-content/liveeditaddon"/>  
          
        <extension dir="${HYBRIS_BIN_DIR}/ext-addon/acceleratorwebservicesaddon"/>  
          
    </extensions>  
</hybrisconfig>  
```

Perform a Build again

``` bash
ant build all
./hybrisserver.sh
```

Got to Hybris Admin console and perform complete initialization
Adding Add On's necessary for OOB B2C electronic store

``` bash
# Include Add On
ant addoninstall -Daddonnames="liveeditaddon" -DaddonStorefront.yacceleratorstorefront="yacceleratorstorefront"
...

[echo] Adding addon 'liveeditaddon' to extensioninfo.xml for 'yacceleratorstorefront'

BUILD SUCCESSFUL
```

If you don't add this plugin it will result in following error while accessing the yaccelerator store, org.apache.jasper.JasperException: An exception occurred processing JSP page /WEB-INF/views/desktop/pages/layout/landingLayout2Page.jsp at line 6

Perform a Build again

``` bash
ant all
./hybrisserver.sh
```

## Store Initialization
Got to Hybris Admin console and perform following steps.
Platform -> update
select only liveeditaddon and click on update.

[https://localhost:9002/yacceleratorstorefront/electronics/en/?site=electronics](https://localhost:9002/yacceleratorstorefront/electronics/en/?site=electronics)

##Setting up custom Store

``` bash
ant modulegen -Dinput.module=accelerator -Dinput.name=myb2cstore -Dinput.package=org.mystore
...
...

   [echo] Next steps:
     [echo] 
     [echo] 1) Add your extension to your /Users/hvadiv/hybris5602/hybris/config/localextensions.xml
     [echo] 
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstorefulfilmentprocess"/>
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstorecore"/>
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstoreinitialdata"/>
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstorefacades"/>
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstoretest"/>
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstorestorefront"/>
     [echo] <extension dir="/Users/hvadiv/hybris5602/hybris/bin/custom/myb2cstore/myb2cstorecockpits"/>
     [echo]
     [echo] 2) Remove the following extensions from your /Users/hvadiv/hybris5602/hybris/config/localextensions.xml
     [echo] yacceleratorfulfilmentprocess,yacceleratorcore,yacceleratorinitialdata,yacceleratorfacades,yacceleratortest,yacceleratorstorefront,yacceleratorcockpits
     [echo]
     [echo] 3) Make sure the applicationserver is stopped before you build the extension the first time.
     [echo] 
     [echo] 4) Perform 'ant' in your hybris/platform directory.
     [echo]
     [echo] 5) Restart the applicationserver
     [echo] 
     [echo] 

BUILD SUCCESSFUL
```

Modified localextensions.xml file in my local setup is as follows.

``` xml
<?xml version="1.0" encoding="UTF-8"?>  
<hybrisconfig xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="resources/schemas/extensions.xsd">  
    <extensions>  
        <!--  
                All extensions located in ${HYBRIS_BIN_DIR}/platform/ext are automatically loaded.  
                More information about how to configure available extensions can be found here : https://wiki.hybris.com/x/nZVzC  
        -->  
                   
        <path dir="${HYBRIS_BIN_DIR}" />  
                                 
        <!-- ext-platform -->  
        <extension name="admincockpit" />  
        <extension name="backoffice" />  
        <extension name="cockpit" />  
        <extension name="hmc" />  
        <extension name="mcc" />  
        <extension name="platformhmc" />  
                                           
        <!-- ext-platform-optional -->  
        <extension name="ldap" />  
        <extension name="lucenesearch" />  
        <extension name="virtualjdbc" />  
                                           
        <!-- ext-cockpit -->  
        <extension name="reportcockpit" />  
                                           
        <!-- ext-commerce -->  
        <extension name="btg" />  
        <extension name="btgcockpit" />  
        <extension name="commercesearch" />  
        <extension name="commercesearchbackoffice" />  
        <extension name="commercesearchhmc" />  
        <extension name="commerceservices" />  
        <extension name="basecommerce" />  
        <extension name="payment" />  
        <extension name="promotions" />  
        <extension name="voucher" />  
        <extension name="customerreview" />  
        <extension name="ticketsystem" />  
        <extension name="solrfacetsearch" />  
        <extension name="solrfacetsearchhmc" />  
        <extension name="oci" />  
        <extension name="wishlist" />  
        <extension name="commercefacades" />  
        <extension name="commercewebservicescommons" />  
                                      
        <!-- ext-content -->  
        <extension name="productcockpit" />  
        <extension name="cms2" />  
        <extension name="cms2lib" />  
        <extension name="cmscockpit" />  
        <extension name="bmecat" />  
        <extension name="bmecathmc" />  
        <extension name="importcockpit" />  
        <extension name="classificationsystems" />  
        <extension name="classificationsystemshmc" />  
                                    
        <!-- ext-channel -->  
        <extension name="cscockpit" />  
        <extension name="mobileoptionals" />  
        <extension name="mobileservices" />  
                                   
        <!-- ext-addon -->  
        <extension name="addonsupport" />  
        <extension name="b2ccheckoutaddon" />  
  
        <!-- ext-print -->  
        <extension name="print" />  
        <extension name="printcockpit" />  
        <extension name="printhmc" />  
  
        <!-- ext-template -->  
        <extension name="ycommercewebservices" />  
        <extension name="ycommercewebserviceshmc" />  
                                        
        <!-- ext-accelerator -->  
        <extension name="acceleratorservices" />  
        <extension name="acceleratorfacades" />  
        <extension name="acceleratorcms" />  
        <extension name="acceleratorstorefrontcommons" />  
          
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstorefulfilmentprocess"/>  
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstorecore"/>  
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstoreinitialdata"/>  
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstorefacades"/>  
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstoretest"/>  
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstorestorefront"/>  
     <extension dir="${HYBRIS_BIN_DIR}/custom/myb2cstore/myb2cstorecockpits"/>  
        <!--extension name="yacceleratorinitialdata" />  
    <extension name="yacceleratorcockpits" />  
    <extension name="yacceleratorfulfilmentprocess" />  
    <extension name="yacceleratorcore" />  
    <extension name="yacceleratorfacades" />  
    <extension name="yacceleratorstorefront" />  
    <extension name="yacceleratortest" /-->  
  
  
    <extension name="electronicsstore" />  
    <extension name="apparelstore" />  
     
    <extension dir="${HYBRIS_BIN_DIR}/ext-addon/b2ccheckoutaddon"/>  
    <extension dir="${HYBRIS_BIN_DIR}/ext-addon/addonsupport"/>  
    <extension dir="${HYBRIS_BIN_DIR}/ext-content/liveeditaddon"/>  
          
        <extension dir="${HYBRIS_BIN_DIR}/ext-addon/acceleratorwebservicesaddon"/>  
          
    </extensions>  
</hybrisconfig>  
```

Perform a Build again

``` bash
ant
./hybrisserver.sh
```

## Store Initialization

Got to Hybris Admin console and perform following steps.
Platform -> update
and start initialization of the new store.
Adding Add On's necessary for OOB B2C electronic store

``` bash
ant addoninstall -Daddonnames="liveeditaddon" -DaddonStorefront.myb2cstorestorefront="myb2cstorestorefront"
ant all
./hybriserver.sh
```

Got to Hybris Admin console and perform following steps.
Platform -> update
select only liveeditaddon and click on update.

[https://localhost:9002/myb2cstorestorefront/electronics/en/?site=electronics](https://localhost:9002/myb2cstorestorefront/electronics/en/?site=electronics)

Temporary Fix
An issue with JSP compilation at runtime while accessing the home page seems to be a defect in 5602 version
Following fix will help resolve this error, watch out for an official fix in the next version
Navigate to generated source code for .\hybris\bin\custom\myb2cstore\myb2cstorestorefront\web\webroot\WEB-INF\common\tld\cmstags.tld
and replace the content with following.
The change is reverting de.hybris.liveeditaddon.cms.tags.LiveeditaddonCMSContentSlotTag back to de.hybris.platform.acceleratorcms.tags2.CMSContentSlotTag.

``` xml
<?xml version="1.0" encoding="UTF-8"?>  
  
  
<taglib xmlns="http://java.sun.com/xml/ns/javaee"  
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd"  
        version="2.1">  
  <tlib-version>1.2</tlib-version>  
  <short-name>cms</short-name>  
  <uri>http://hybris.com/tld/cmstags</uri>  
  
  
  <tag>  
  <description>Iterates over the components in a CMS content slot for a position in the current page</description>  
  <name>pageSlot</name>  
  <tag-class>de.hybris.platform.acceleratorcms.tags2.CMSContentSlotTag</tag-class>  
  <body-content>JSP</body-content>  
  <attribute>  
  <description>the position of the content slot in the CMS page</description>  
  <name>position</name>  
  <required>true</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the var name to access the content element</description>  
  <name>var</name>  
  <required>true</required>  
  <rtexprvalue>false</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the limit on the number of components to render in this slot</description>  
  <name>limit</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the HTML element to generate for this slot</description>  
  <name>element</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <!-- Any other attributes specified are written onto the element -->  
  <dynamic-attributes>true</dynamic-attributes>  
  </tag>  
  
  
  <tag>  
  <description>Iterates over the components in a CMS content slot given the UID of the slot</description>  
  <name>globalSlot</name>  
  <tag-class>de.hybris.platform.acceleratorcms.tags2.CMSContentSlotTag</tag-class>  
  <body-content>JSP</body-content>  
  <attribute>  
  <description>the uid of content slot</description>  
  <name>uid</name>  
  <required>true</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the var name to access the content element</description>  
  <name>var</name>  
  <required>true</required>  
  <rtexprvalue>false</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the limit on the number of components to render in this slot</description>  
  <name>limit</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the HTML element to generate for this slot</description>  
  <name>element</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <!-- Any other attributes specified are written onto the element -->  
  <dynamic-attributes>true</dynamic-attributes>  
  </tag>  
  
  
  <!-- The 'slot' tag is deprecated -->  
  <!-- <tag>  
  <description>Iterates over a CMS content slot</description>  
  <name>slot</name>  
  <tag-class>de.hybris.platform.acceleratorcms.tags2.CMSContentSlotTag</tag-class>  
  <body-content>JSP</body-content>  
  <attribute>  
  <description>the content slot</description>  
  <name>contentSlot</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the uid of content slot</description>  
  <name>uid</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the position of content slot</description>  
  <name>position</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the var name to access the content element</description>  
  <name>var</name>  
  <required>true</required>  
  <rtexprvalue>false</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the limit on the number of components to render in this slot</description>  
  <name>limit</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  </tag> -->  
  
  
  <tag>  
  <description>Renders a component by calling it's controller</description>  
  <name>component</name>  
  <tag-class>de.hybris.platform.acceleratorcms.tags2.CMSComponentTag</tag-class>  
  <body-content>JSP</body-content>  
  <attribute>  
  <description>the uid of the component</description>  
  <name>uid</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the component</description>  
  <name>component</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the parent component</description>  
  <name>parentComponent</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>Set to true in order to evaluate restrictions on this component, default value false. Note that restrictions are typically evaluated by the slot tag.</description>  
  <name>evaluateRestriction</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <attribute>  
  <description>the HTML element to generate for this component</description>  
  <name>element</name>  
  <required>false</required>  
  <rtexprvalue>true</rtexprvalue>  
  </attribute>  
  <!-- Any other attributes specified are written onto the element -->  
  <dynamic-attributes>true</dynamic-attributes>  
  </tag>  
 </taglib>  
 ```

## Reference
[https://wiki.hybris.com/display/release5/Installation](https://wiki.hybris.com/display/release5/Installation)
