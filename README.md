# Generator of MySQL Stored Procedures
 
This program lets you automatize the insert stored procedures generation. 
 

## Motivation
 
Basically, not losing too much time on easy tasks.  

## Functionality of the software

### Funcions:
1) getTableInfo: Takes the standarized code of MySQL and generate a list with the format: 

        [   tableName, 

            [mandatoryColumn1 mandatoryColumnMetaData1, mandatoryColumn2 mandatoryColumnMetaDataN, ... , mandatoryColumn1 mandatoryColumnMetaDataN],
            
            [optionalColumn1 optionalColumnMetaData1, optionalColumn2 optionalColumnMetaDataN, ... , optionalColumn1 mandatoryColumnMetaDataN],

            [primaryKeyColumn1 primaryKeyColumnMetaData1, primaryKeyColumn2 primaryKeyColumnMetaData2, ... , primaryKeyColumnN primaryKeyColumnMetaDataN]
        ]

2) (register|modify)Table: Generates the Stored Procedure, based on the output data structure of getTableInfo (only with the mandatory columns).

3) combinatoryTableInfoStart: Using the output data structure of getTableInfo, calls registerTable with all the possible combinations of optional columns. 

    If you have [idKey1, idKey2, idKey3, idKey4], as OPTIONAL columns.

    Then you may see this function input as [1,1,1] and generation the calls:

        registerTable([mandatoryColumns + [0,0,0]])
        
        modifyTable([mandatoryColumns + [0,0,0] + primaryKeyColumns])

        registerTable([mandatoryColumns + [0,0,1]])

        modifyTable([mandatoryColumns + [0,0,1] + primaryKeyColumns])

        registerTable([mandatoryColumns + [0,1,0]])

        modifyTable([mandatoryColumns + [0,1,0] + primaryKeyColumns])

        registerTable([mandatoryColumns + [0,1,1]])

        modifyTable([mandatoryColumns + [0,1,1] + primaryKeyColumns])

        registerTable([mandatoryColumns + [1,0,0]])

        modifyTable([mandatoryColumns + [1,0,0] + primaryKeyColumns])

        registerTable([mandatoryColumns + [1,0,1]])

        modifyTable([mandatoryColumns + [1,0,1] + primaryKeyColumns])

        registerTable([mandatoryColumns + [1,1,0]])

        modifyTable([mandatoryColumns + [1,1,0] + primaryKeyColumns])

        registerTable([mandatoryColumns + [1,1,1]])

        modifyTable([mandatoryColumns + [1,1,1] + primaryKeyColumns])

4) processDataBaseFile: Process the file with the database code, split it, and calls getTableInfo to structure its data. 

## Global Variables (if using files)

projectName: The name of the project (which would be use to create a new directory on the current directory).
fileCounter: Don't touch it. 
dataBaseFileLocation: Specify the location of the MySQL code generating the tables (using the FORWARD ENGINEERING - WORKBENCH Standard).
dataBaseName: Specify the name of the DataBase on the File. 

## Use

### 1) Just a table: 
You take the table code, changing manually the change of line by "\n", for example: 

z="\n-- -----------------------------------------------------\n-- Table `ADADB`.`Habilidad`\n-- -----------------------------------------------------\nCREATE TABLE IF NOT EXISTS `ADADB`.`Habilidad` (\n  `idHabilidad` MEDIUMINT UNSIGNED NOT NULL AUTO_INCREMENT,\n  `idCategoriaHabilidad` SMALLINT UNSIGNED NOT NULL,\n  `nombreHabilidad` NVARCHAR(45) NOT NULL,\n  `descripcionHabilidad` NVARCHAR(450) NOT NULL,\n  PRIMARY KEY (`idHabilidad`, `idCategoriaHabilidad`),\n  INDEX `fk_Habilidad_CategoriaHabilidad1_idx` (`idCategoriaHabilidad` ASC),\n  UNIQUE INDEX `idHabilidad_UNIQUE` (`idHabilidad` ASC),\n  CONSTRAINT `fk_Habilidad_CategoriaHabilidad1`\n    FOREIGN KEY (`idCategoriaHabilidad`)\n    REFERENCES `ADADB`.`CategoriaHabilidad` (`idCategoriaHabilidad`)\n    ON DELETE NO ACTION\n    ON UPDATE NO ACTION)\nENGINE = InnoDB;"

And then call the function: combinatoryTableInfoStart(getTableInfo(z))

### 2) The file: 
Be sure that global variables were correctly defined.
And just call: processDataBaseFile()

## Contributors
 
 * [Eduardo Chavarría Rey](https://github.com/echavrey/)