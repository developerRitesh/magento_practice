# magento_practice

1. create module : etc/module.xml registration.php componentRegistrat:Register()
2. create theme : theme.xml(parent,mediafile) registration.php etc/view.xml (contain product base small thumbnail , product media gallery image res , vars)
3. create languge pack : language.xml(code,vendor,package) (app/i18n) ,registration.php , hi_IN.csv (in root with registration.php) bin/mag i18n:collect-phrases -o "var/trans_raw.csv"
4. search criteria builder
   sci = scb->addFilter(f1,v1,eq);
   sci = scb->addFilter([f1,v1,eq,f2,v2,eq]); //or
   sci = scb->addFilters(fb1,fb2); (\Magento\Framework\Api\filterBuilder->setfield->setvalue->setcondition)
   sci = scb->filterGroupBuilder([FG1,FG2]); (FG1 = filterGroupBuilder->setFilters([$f1, $f2]) )
   sci = scb->setSortOrders([m/f/api/sortOrder]); //or
   join di.xml
   type=JoinProcessor
   argument_name custom_join-> customjoinclass  
   customjoinclass-> applyfunction($collection) {$collection->getSelect()->joinLeft(}
5. plugin di.xml
     <type name=originamclass>
        <plugin type="ourclass" disabled sortorder>
     ourclass
   before(Originalclass $subject,$arguments)
   after(Originalclass $subject,$result,$arghuments)
   around(Originalclass $subject,callable $proceed,$arguments){$result = $proceed(...$args);}
6.events (Magento\Framework\Event\Manager->dispatch('event_name', ['info' => $text])) observers ,
   observer : events.xml
   <event name="event_name"> //created with dispatch
      <observer instance="myclass"> myclass implement \Magento\Framework\Event\ObserverInterface -> execute(Magento\Framework\Event\Observer $observer){$order = $observer->getEvent()->getOrder();}
7. etc/email_templates.xml transportbuilder->setTemplateIdentifier("template_id")->setTemplateVars()->getTransport();$transport->sendMessage();
8. db_schema.xml s:u --safe-mode=1 , if dont want to drop the schema setup:db-declaration:generate-whitelist to generate the schema.
        <constraint type="primary"> 
            <column name="block_id"/>
        </constraint>
        <constraint type="foreign" table="cms_block_store" column="block_id"
         referenceTable="cms_block" referenceColumn="block_id" onDelete="CASCADE"/>
9. isolated security patch : patch -p1 -i VULN-31609_2.4.X.patch
10. data patch Setup\Patch\Data\CustomClass extends Magento\Framework\Setup\Patch\PatchInterface {apply(),getAliases(),getDependencies(),getVersion(),revert() }
11. EAV model -> database design pattern for entites such as customer and product to support infinite number of attribute without altering core database schema. we divide the entity into 5-6 table based on the data type , for example catalog_product_entity_[datatype]
12. index.php -> m/f/app/http->launch() call frontcontrollers->dispatch which loads routerlist match function which loops every known router available
    base route , cms route , url redirect route , robots.txt route , default route (404)
13. custom router : di.xml routerlist class-> custom class extend m/f/app/RouterInterface -> match(request)
14. etc/crontab.xml <group id="default"> (etc/cron_groups.xml for new group <group id="<group_name>">)
             <job name="sales_clean_quotes" instance="Magento\Sales\Cron\CleanExpiredQuotes" method="execute">
                   <schedule>0 0 * * *</schedule>
                 </job>
15.console command main class : CommandList class di.xml <type Magento\Framework\Console\CommandList>
                               <argument name command -> customclass extend symphony/command {execute(S/C/InputInterface $input, OutputInterface $output)}
16.di.xml

   overwrite class
    <preference for="OM\AccessaryPincode\Api\AccessorypincodeRepositoryInterface" type="OM\AccessaryPincode\Model\AccessorypincodeRepository"/>
   
   change class constructor argument 
    <virtualType name="virtualname" type="customclass">
        <arguments>
            <argument name="prefix" xsi:type="string">SMS Alert: </argument>
            <argument name="logger" xsi:type="object">Magento\Framework\Logger\Monolog</argument>
        </arguments>
    </virtualType>
    declare plugins 
    <type name=originamclass>
       <plugin type="ourclass" disabled sortorder>
     </type>
     
    <type name=originamclass>
       <arguments>
           <argument name="customValue" xsi:type="string">Hello</argument>
       </arguments>
     </type>
17. addd custom js  in phtml , declarative syntax 
   <button id="my-button"></button>
    <script type="text/x-magento-init">
    {
        "#my-button": {
            "Vendor_Module/js/my-button": {
                "message": "Hello"
            }
        }
    }
    </script>
    my-button.js
    
    return function (config, element) {
        //config is function argument passed and element is actual html element in which this js is being targetted
        console.log(config.message);
    }; // we also use data-mage-init for acheiving this inline
    imperative syntax
    app/design/frontend/{Vendor}/{theme}/requirejs-config.js
      var config = {
          map: {
              '*': {
                  myscript: 'js/myfile'
              }
          }
      };
18. knockout js : magento_ui/js/core/app , data-bind="scope:mycomponent" <script x-magento-init> component, config {dynamic variables}
    ko.computed(),ko.pureComputed(),subscribe() , imports, links 
    define([
      'uiComponent',
       'uiRegistry'
   ], function (Component, registry) {
    'use strict';

    return Component.extend({
         defaults: {
            availableCouponList: ko.observableArray([]),
            isLoading: ko.observable(true),
                        
        },
        updateCityField: function (newCity) {
            registry.get('checkout.steps.shipping-step.shippingAddress.shipping-address-fieldset.city', function (cityComponent) {
                cityComponent.value(newCity);
            });
        }
    });
});  
19. indexer: indexer.xml class Test implements \Magento\Framework\Indexer\ActionInterface
20. rest api , etc/wepapi.xml <route url="vi/customer/check" method="get"><service class="mycustominterface" method="myfunction"><resource ref=""self
21. graphql : etc/schema.graphqls query(input1,input2):myresolver@resolver  extend Magento\Framework\GraphQl\Query\ResolverInterface resolve($field,$context,$info,$value,$args) cacheintentityinterface, X-Magento-Tags ,X-Magento-Cache-Id (customer gorup wise caching) ,  
for varnish X-Magento-Cache-Debug: HIT
22. eav attribute : implements DataPatchInterface {Magento\Eav\Setup\EavSetupFactory->
   $eavSetup->addAttribute(Category::ENTITY, 'category_recommendation', [
            'type' => 'int',
            'label' => 'Is Recommended',
            'input' => 'select',
            'source' => Boolean::class,
            'sort_order' => 26,
            'global' => ScopedAttributeInterface::SCOPE_STORE,
            'group' => 'General Information'
        ]);
24. customer_account.xml <referenceBlock name="customer_account_navigation"><argument name="path" xsi:type="string">routename/customer/index</argument>
25. private content : <sections.xml Magento\Customer\CustomerData\SectionPoolInterface  <item name="custom_section" class CustomerData/customclass  extend implements Magento\Customer\CustomerData\SectionSourceInterface\SectionSourceInterface getSectionData();
define([
	'uiComponent',
	'Magento_Customer/js/customer-data'
], function (Component, customerData) {
	'use strict';

	return Component.extend({
    	initialize: function () {
        	this._super();
        	this.customsection = customerData.get('custom_section');
    	}
	});

});
26. rabbit mq : see page 
27 . shiping method : Magento\Shipping\Model\Carrier\AbstractCarrier collectRates() config.xml source 
28 . checkout : layoutprocessor file , create plugin afterProcess($subject,$jsLayout) , this will add field , then we have to create extension attribute Magento\Quote\Api\Data\AddressInterface and create  set-shipping-information-mixin.js , 


   


   
