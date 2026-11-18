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
     



   


   
