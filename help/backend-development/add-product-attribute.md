---
title: Skapa ett produktattribut
description: Skapa en sida som returnerar json med en parameter.
kt: 14131
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, User
level: Beginner, Intermediate
exl-id: 98257e62-b23d-4fa9-a0eb-42e045c53195
source-git-commit: 88b957a33d6061c8053e598248fcbfff5cf0f010
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Skapa ett produktattribut

Att lägga till ett produktattribut är en av de vanligaste åtgärderna i [!DNL Commerce]. Attribut är ett kraftfullt sätt att lösa många praktiska uppgifter som rör en produkt. Det är en enkel process att lägga till ett attribut av listrutetyp i en produkt.

I den här videon:

- Lägg till ett attribut med namnet klädesplagg med möjliga värden: bomull, läder, silke, denim, päls och ull
- Gör det här attributet synligt på produktvysidan, i fet stil
- Tilldela den till attributuppsättningen Standard och lägg till en begränsning
- Lägg till det nya attributet

## Vem är den här videon till?

- Utvecklare som är nya inom e-handel och som behöver lära sig att skapa ett produktattribut programmatiskt

## Videoinnehåll

>[!VIDEO](https://video.tv.adobe.com/v/35789?quality=12&learn=on)

## Kodexempel

Skapa först de mappar, XML- och PHP-filer som behövs:

- app/code/Learning/ClothingMaterial/registration.php
- app/code/Learning/ClothingMaterial/etc/module.xml
- app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php
- app/code/Learning/ClothingMaterial/Setup/installData.php

### app/code/Learning/ClothingMaterial/registration.php

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Learning_ClothingMaterial',
    __DIR__);
```

### app/code/Learning/ClothingMaterial/etc/module.xml

>[!NOTE]
>
>Om modulen använder deklarativt schema, och de flesta har sedan 2.3.0, bör du utelämna setup_version. Om du har äldre projekt kan du se hur den här metoden används.  Mer information finns på [developer.adobe.com](https://developer.adobe.com/commerce/php/development/build/component-name/#add-a-modulexml-file){target="_blank"}.


```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Learning_ClothingMaterial" setup_version="0.0.1"/>
</config>
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Backend;

use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\Backend\AbstractBackend;
use Magento\Framework\Exception\LocalizedException;

class Material extends AbstractBackend
{
    /**
     * @param Product @object
     * @throws LocalizedException
     */
    public function validate($object)
    {
        $value =$object->getData($this->getAttribute()->getAttributeCode());
        // Be sure to validate that your ID number it is likely to be different

        if (($object->getAttributeSetId() == 9) && ($value == 'fur')) {
            throw new LocalizedException(__('Bottoms cannot be fur'));
        }
    }
}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Frontend;

use Magento\Eav\Model\Entity\Attribute\Frontend\AbstractFrontend;
use Magento\Framework\DataObject;

class Material extends AbstractFrontend
{

    public function getValue(DataObject $object): string
    {
        $value = $object->getData($this->getAttribute()->getAttributeCode());
        return "<b>$value</b>";
    }

}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Source;

use Magento\Eav\Model\Entity\Attribute\Source\AbstractSource;

class Material extends AbstractSource
{
    /**
     * Get all options
     *
     * @retrun array
     */
    public function getAllOptions(): array
    {
       if(!$this->_options){
           $this->_options = [
               ['label'    => __('Cotton'),     'value' => 'cotton'],
               ['label'    => __('Leather'),    'value' => 'leather'],
               ['label'    => __('Silk'),       'value' => 'silk'],
               ['label'    => __('Fur'),        'value' => 'fur'],
               ['label'    => __('Wool'),       'value' => 'wool'],
           ];
       }
       return $this->_options;
    }
}
```

### app/code/Learning/ClothingMaterial/Setup/installData.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Setup;

use Learning\ClothingMaterial\Model\Attribute\Frontend\Material as Frontend;
use Learning\ClothingMaterial\Model\Attribute\Source\Material as Source;
use Learning\ClothingMaterial\Model\Attribute\Backend\Material as Backend;
use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface;
use Magento\Framework\Setup\InstallDataInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class InstallData implements InstallDataInterface
{
    protected $eavSetupFactory;

    public function __construct(
        \Magento\Eav\Setup\EavSetupFactory $eavSetupFactory
    )
    {
        $this->eavSetupFactory = $eavSetupFactory;
    }

    /**
     * @param ModuleDataSetupInterface $setup
     * @param ModuleContextInterface $context
     * {@inheritDoc}
     *
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     * @suppressWarnings(PHPMD.ExessiveMethodLength)
     * @SuppressWarnings(PHPMD.NPathComplexity)
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $eavSetup = $this->eavSetupFactory->create();
        $eavSetup->addAttribute(
            Product::ENTITY,
            'clothing_material',
            [
                'group'         => 'General',
                'type'          => 'varchar',
                'label'         => 'Clothing Material',
                'input'         => 'select',
                'source'        => Source::class,
                'frontend'      => Frontend::class,
                'backend'       => Backend::class,
                'required'      => false,
                'sort_order'    => 50,
                'global'        => ScopedAttributeInterface::SCOPE_GLOBAL,
                'is_used_in_grid'               => false,
                'is_visible_in_grid'            => false,
                'is_filterable_in_grid'         => false,
                'visible'                       => true,
                'is_html_allowed_on_frontend'   => true,
                'visible_on_front'              => true,
            ]
        );
    }
}
```

## Användbara resurser

[Skapa ett produktattribut](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/add-product-attribute.html)

[Lägg till ett anpassat textfältsattribut](https://developer.adobe.com/commerce/php/tutorials/admin/custom-text-field-attribute/)
