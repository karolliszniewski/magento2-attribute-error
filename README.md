# magento2-attribute-error


In /Blog/Model/Post/Gallery/CreateHandler.php:

Update the $mediaAttributesWithLabels array to include all the necessary image attributes:
```private $mediaAttributesWithLabels = [
    'image',
    'small_image',
    'thumbnail',
    'swatch_image',
    'magefan_og_image',
    'product_badges',
    'sidebar_gallery_1',
    'sidebar_gallery_2',
    'sidebar_gallery_3',
    'sidebar_gallery_4'
];
```


In `/Blog/Controller/Adminhtml/Post/Save.php`:

Update the $fields array to match the $mediaAttributesWithLabels array:
```$fields = [
    'image',
    'small_image',
    'thumbnail',
    'swatch_image',
    'magefan_og_image',
    'product_badges',
    'sidebar_gallery_1',
    'sidebar_gallery_2',
    'sidebar_gallery_3',
    'sidebar_gallery_4'
];
```


In `/Blog/Helper/Post.php`:

Add getter methods for all the image attributes:
```php
public function getImage($post)
{
    return $post->getData('image');
}

public function getSmallImage($post)
{
    return $post->getData('small_image');
}

public function getThumbnail($post)
{
    return $post->getData('thumbnail');
}

// Add similar getter methods for the other image attributes
```


In `/Blog/etc/adminhtml/di.xml`:

Ensure that the imageTypes configuration includes all the necessary image attributes:
```xml
<item name="swatch_image" xsi:type="array">
    <item name="title" xsi:type="string" translatable="true">Swatch Image</item>
    <item name="attribute" xsi:type="string">swatch_image</item>
</item>
<item name="magefan_og_image" xsi:type="array">
    <item name="title" xsi:type="string" translatable="true">OG Image</item>
    <item name="attribute" xsi:type="string">magefan_og_image</item>
</item>
<!-- Add similar configurations for the other image attributes -->
```


`/Blog/Block/Post/View/Gallery.php`:

Update the getGalleryImagesJson() method to use the standardized image attribute names:
```php
$imageItem = new DataObject(
    [
        'thumb' => $image->getData('small_image_url'),
        'img' => $image->getData('medium_image_url'),
        'full' => $image->getData('large_image_url'),
        'caption' => $image->getLabel() ?: $this->getPost()->getName(),
        'position' => $image->getData('position'),
        'isMain' => $this->isMainImage($image),
        'type' => $mediaType !== null ? str_replace('external-', '', $mediaType) : '',
        'videoUrl' => $image->getVideoUrl(),
    ]
);
```


In `/Blog/Ui/DataProvider/Post/Form/Modifier/Images.php`:

Ensure that the attribute names in the Images class match the standardized image attribute names:
```php
const CODE_IMAGE = 'image';
const CODE_SMALL_IMAGE = 'small_image';
const CODE_THUMBNAIL = 'thumbnail';
const CODE_SWATCH_IMAGE = 'swatch_image';
const CODE_MAGEFAN_OG_IMAGE = 'magefan_og_image';
```
// Add similar constants for the other image attributes



In `Aware/Blog/Setup/CategorySetup.php`:

Verify that the image attribute definitions are correct and consistent with the rest of the module.


In `Aware/Blog/Setup/Patch/Data/UpdatePostAttributes.php`:

Ensure that the attribute updates are aligned with the standardized image attribute names.


In `/Blog/view/adminhtml/ui_component/design_config_form.xml`:

Verify that the thumbnail-related settings are properly configured and use the correct attribute names.



By making these changes, you should be able to consolidate and standardize the handling of the various image attributes in the Aware Blog module. This will ensure consistency across the codebase and fix the issues you were experiencing.
Please let me know if you have any further questions or need additional assistance.
