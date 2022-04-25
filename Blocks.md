
## "Global" vs "content" blocks
There are two types of blocks within the system:
- [Global blocks](/Blocks/Global-Blocks) (i.e. Header, mobile menu and footer)
- [Content blocks](/Blocks/Content-Blocks) (e.g. Hero, side by side, timeline)

## "Backend" and "frontend" files
Each of these will have a:
- "Backend file" (e.g. [CONT001.php](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/includes/MWB/Block/CONT/CONT001.php)) will contain a description, ACF fields, data retrieval and passing of data to the "frontend file"
- "Frontend file" (e.g. [CONT001.twig](https://gitlab.com/visix/wordpress/themes/mwb-modules-base/-/blob/master/resources/views/block/CONT/CONT001.twig)) is a [Twig](https://twig.symfony.com/) file, where all the hard work should've already have been done, and it just needs to render it all.

## Alteration & Extension
Every block can be altered and new ones can be created too. You can also alter the available selection of blocks so that you can restrict the choices just to what has been designed as an example.

To alter an existing block from the parent theme, simply copy the file to your child theme, this file will now be used instead. Although you can override just one of the two files for each block, it is recommended you override both in case the base theme later updates the block, and your version is now got half of the expected code. 