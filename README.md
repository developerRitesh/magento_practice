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



   
