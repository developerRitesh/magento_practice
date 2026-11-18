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
5. plugin : di.xml
     <type=originamclass>
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
10. EAV model -> database design pattern for entites such as customer and product to support infinite number of attribute without altering core database schema. we divide the entity into 5-6 table based on the data type , for example catalog_product_entity_[datatype]
11. index.php -> m/f/app/http->launch() call frontcontrollers->dispatch which loads routerlist match function which loops every known router available
    base route , cms route , url redirect route , robots.txt route , default route (404)
12. custom router : di.xml routerlist class-> custom class extend m/f/app/RouterInterface -> match(request)



   


   
