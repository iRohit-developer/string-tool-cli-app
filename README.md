# String-Tool-CLI-App Using Php Symfony Console
## Create a new directory for our project and `cd` into it: 
```mkdir string-tool-cli-app && cd string-tool-cli-app``` - in cmd

## Require the Console Component into our project using Composer.
you can download the composer from https://getcomposer.org/download
There you will find Command-line installation
To quickly install Composer in the current directory, run the following script in your terminal. To automate the installation, use the guide on installing Composer programmatically. 

Open your bash or cmd in your project path
-Then starting executing codes line by line
```php
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```
After composer installed successfully, execute below code
```php
composer require symfony/console
```
3)Now create an entry point to our application. A PHP extension is not necessary as we are going to make this file executable and specify the environment in the file itself. 
```touch StringTool```
```chmod +X StringTool```
...lets create a simple hello world before getting into the main scenario.
vim StringTool - after execution this code we get editor there just enter i for insert mode and write the code
Everyone get stuck at intrepreter error so here is the solution - ``` #!/usr/bin/env php ```
```
#!/usr/bin/env php
<?php

if (php_sapi_name() !== 'cli') {
    exit;
}

echo "Hello World\n";
```
Now run the application with:
```./StringTool```
We should see a Hello World as output.
Lets Jump into the main scenario...........

4)Remove code in StringTool, lets add this code
```
#!/usr/bin/env php
<?php
require __DIR__.'/vendor/autoload.php';

use Symfony\Component\Console\Application;
use Symfony\Component\Console\Input\InputArgument;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Input\InputOption;
use Symfony\Component\Console\Output\OutputInterface;

(new Application('String Tool', '1.0.0'))
      ->register('string')

      ->addArgument('name', InputArgument::OPTIONAL, 'Name of the string')

      ->setCode(function (InputInterface $input, OutputInterface $output) {
              
        $name = $input->getArgument('name');
        
        $letter_count = 0;
        $slu = '';
        for ($i=0; $i<strlen($name); $i++) {
            if (!preg_match('![a-zA-Z]!', $name[$i])) {
                $slu .= $name[$i];
            } else if ($letter_count++ & 1) {
                $slu .= strtoupper($name[$i]);
            } else {
                $slu .= $name[$i];
            }
        }
        
        $stringTextLower = strtolower($name);
        $stringSplitToOne = str_split($stringTextLower,"1");
        $newString = implode(",",$stringSplitToOne);
        
        $data = str_getcsv($newString);
        $fp=fopen('file.csv','w');
        fputcsv($fp,$data);
        fclose($fp);
        
        if (!empty($name)) {
            return $output->writeln([
                    "<info>".strtoupper($name)."</info>",
                    "<info>".$slu."</info>",
                    "CSV created!",
                    ]);
        } else {
            return $output->writeln("<error>Enter string name</error>");
        }
      })
      ->getApplication()
      ->run();
```      
That's it. 
Run the ./StringTool in command line there we will find our StringTool info screen.
After that ./StringTool command [input arguments]
type - ```./StringTool string "hello world"```
Finally we will get output like...
```
HELLO WORLD
hElLo WoRlD
CSV created!
```

   
