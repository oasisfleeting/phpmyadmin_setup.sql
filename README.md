# phpmyadmin_setup.sql
This is the phpmyadmin setup script for extended configuration.  


-- --------------------------------------------------------
-- SQL Commands to set up the pmadb as described in the documentation.
--
-- This file is meant for use with MySQL 5 and above!
--
-- This script expects the user pma to already be existing. If we would put a
-- line here to create him too many users might just use this script and end
-- up with having the same password for the controluser.
--
-- This user "pma" must be defined in config.inc.php (controluser/controlpass)
--
-- Please don't forget to set up the tablenames in config.inc.php
--

-- --------------------------------------------------------

--
-- Database : `phpmyadmin`
--
CREATE DATABASE IF NOT EXISTS `phpmyadmin`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;
USE phpmyadmin;

-- --------------------------------------------------------

--
-- Privileges
--
-- (activate this statement if necessary)
-- GRANT SELECT, INSERT, DELETE, UPDATE ON `phpmyadmin`.* TO
--    'pma'@localhost;

-- --------------------------------------------------------

--
-- Table structure for table `pma_bookmark`
--

CREATE TABLE IF NOT EXISTS `pma_bookmark` (
  `id` int(11) NOT NULL auto_increment,
  `dbase` varchar(255) NOT NULL default '',
  `user` varchar(255) NOT NULL default '',
  `label` varchar(255) COLLATE utf8_general_ci NOT NULL default '',
  `query` text NOT NULL,
  PRIMARY KEY  (`id`)
)
  COMMENT='Bookmarks'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_column_info`
--

CREATE TABLE IF NOT EXISTS `pma_column_info` (
  `id` int(5) unsigned NOT NULL auto_increment,
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `column_name` varchar(64) NOT NULL default '',
  `comment` varchar(255) COLLATE utf8_general_ci NOT NULL default '',
  `mimetype` varchar(255) COLLATE utf8_general_ci NOT NULL default '',
  `transformation` varchar(255) NOT NULL default '',
  `transformation_options` varchar(255) NOT NULL default '',
  PRIMARY KEY  (`id`),
  UNIQUE KEY `db_name` (`db_name`,`table_name`,`column_name`)
)
  COMMENT='Column information for phpMyAdmin'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_history`
--

CREATE TABLE IF NOT EXISTS `pma_history` (
  `id` bigint(20) unsigned NOT NULL auto_increment,
  `username` varchar(64) NOT NULL default '',
  `db` varchar(64) NOT NULL default '',
  `table` varchar(64) NOT NULL default '',
  `timevalue` timestamp NOT NULL,
  `sqlquery` text NOT NULL,
  PRIMARY KEY  (`id`),
  KEY `username` (`username`,`db`,`table`,`timevalue`)
)
  COMMENT='SQL history for phpMyAdmin'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_pdf_pages`
--

CREATE TABLE IF NOT EXISTS `pma_pdf_pages` (
  `db_name` varchar(64) NOT NULL default '',
  `page_nr` int(10) unsigned NOT NULL auto_increment,
  `page_descr` varchar(50) COLLATE utf8_general_ci NOT NULL default '',
  PRIMARY KEY  (`page_nr`),
  KEY `db_name` (`db_name`)
)
  COMMENT='PDF relation pages for phpMyAdmin'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_recent`
--

CREATE TABLE IF NOT EXISTS `pma_recent` (
  `username` varchar(64) NOT NULL,
  `tables` text NOT NULL,
  PRIMARY KEY (`username`)
)
  COMMENT='Recently accessed tables'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_uiprefs`
--

CREATE TABLE IF NOT EXISTS `pma_table_uiprefs` (
  `username` varchar(64) NOT NULL,
  `db_name` varchar(64) NOT NULL,
  `table_name` varchar(64) NOT NULL,
  `prefs` text NOT NULL,
  `last_update` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`username`,`db_name`,`table_name`)
)
  COMMENT='Tables'' UI preferences'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_relation`
--

CREATE TABLE IF NOT EXISTS `pma_relation` (
  `master_db` varchar(64) NOT NULL default '',
  `master_table` varchar(64) NOT NULL default '',
  `master_field` varchar(64) NOT NULL default '',
  `foreign_db` varchar(64) NOT NULL default '',
  `foreign_table` varchar(64) NOT NULL default '',
  `foreign_field` varchar(64) NOT NULL default '',
  PRIMARY KEY  (`master_db`,`master_table`,`master_field`),
  KEY `foreign_field` (`foreign_db`,`foreign_table`)
)
  COMMENT='Relation table'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_coords`
--

CREATE TABLE IF NOT EXISTS `pma_table_coords` (
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `pdf_page_number` int(11) NOT NULL default '0',
  `x` float unsigned NOT NULL default '0',
  `y` float unsigned NOT NULL default '0',
  PRIMARY KEY  (`db_name`,`table_name`,`pdf_page_number`)
)
  COMMENT='Table coordinates for phpMyAdmin PDF output'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_info`
--

CREATE TABLE IF NOT EXISTS `pma_table_info` (
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `display_field` varchar(64) NOT NULL default '',
  PRIMARY KEY  (`db_name`,`table_name`)
)
  COMMENT='Table information for phpMyAdmin'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_designer_coords`
--

CREATE TABLE IF NOT EXISTS `pma_designer_coords` (
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `x` INT,
  `y` INT,
  `v` TINYINT,
  `h` TINYINT,
  PRIMARY KEY (`db_name`,`table_name`)
)
  COMMENT='Table coordinates for Designer'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_tracking`
--

CREATE TABLE IF NOT EXISTS `pma_tracking` (
  `db_name` varchar(64) NOT NULL,
  `table_name` varchar(64) NOT NULL,
  `version` int(10) unsigned NOT NULL,
  `date_created` datetime NOT NULL,
  `date_updated` datetime NOT NULL,
  `schema_snapshot` text NOT NULL,
  `schema_sql` text,
  `data_sql` longtext,
  `tracking` set('UPDATE','REPLACE','INSERT','DELETE','TRUNCATE','CREATE DATABASE','ALTER DATABASE','DROP DATABASE','CREATE TABLE','ALTER TABLE','RENAME TABLE','DROP TABLE','CREATE INDEX','DROP INDEX','CREATE VIEW','ALTER VIEW','DROP VIEW') default NULL,
  `tracking_active` int(1) unsigned NOT NULL default '1',
  PRIMARY KEY  (`db_name`,`table_name`,`version`)
)
  COMMENT='Database changes tracking for phpMyAdmin'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_userconfig`
--

CREATE TABLE IF NOT EXISTS `pma_userconfig` (
  `username` varchar(64) NOT NULL,
  `timevalue` timestamp NOT NULL,
  `config_data` text NOT NULL,
  PRIMARY KEY  (`username`)
)
  COMMENT='User preferences storage for phpMyAdmin'
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;




-- --------------------------------------------------------
-- SQL Commands to set up the pmadb as described in the documentation.
--
-- This file is meant for use with Drizzle 2011.03.13 and above!
--
-- This script expects that you take care of database permissions.
--
-- Please don't forget to set up the tablenames in config.inc.php
--

-- --------------------------------------------------------

--
-- Database : `phpmyadmin`
--
CREATE DATABASE IF NOT EXISTS `phpmyadmin`
  COLLATE utf8_bin;
USE phpmyadmin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_bookmark`
--

CREATE TABLE IF NOT EXISTS `pma_bookmark` (
  `id` int(11) NOT NULL auto_increment,
  `dbase` varchar(255) NOT NULL default '',
  `user` varchar(255) NOT NULL default '',
  `label` varchar(255) COLLATE utf8_general_ci NOT NULL default '',
  `query` text NOT NULL,
  PRIMARY KEY  (`id`)
)
  COMMENT='Bookmarks'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_column_info`
--

CREATE TABLE IF NOT EXISTS `pma_column_info` (
  `id` int(5) NOT NULL auto_increment,
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `column_name` varchar(64) NOT NULL default '',
  `comment` varchar(255) COLLATE utf8_general_ci NOT NULL default '',
  `mimetype` varchar(255) COLLATE utf8_general_ci NOT NULL default '',
  `transformation` varchar(255) NOT NULL default '',
  `transformation_options` varchar(255) NOT NULL default '',
  PRIMARY KEY  (`id`),
  UNIQUE KEY `db_name` (`db_name`,`table_name`,`column_name`)
)
  COMMENT='Column information for phpMyAdmin'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_history`
--

CREATE TABLE IF NOT EXISTS `pma_history` (
  `id` bigint(20) NOT NULL auto_increment,
  `username` varchar(64) NOT NULL default '',
  `db` varchar(64) NOT NULL default '',
  `table` varchar(64) NOT NULL default '',
  `timevalue` timestamp NOT NULL,
  `sqlquery` text NOT NULL,
  PRIMARY KEY  (`id`),
  KEY `username` (`username`,`db`,`table`,`timevalue`)
)
  COMMENT='SQL history for phpMyAdmin'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_pdf_pages`
--

CREATE TABLE IF NOT EXISTS `pma_pdf_pages` (
  `db_name` varchar(64) NOT NULL default '',
  `page_nr` int(10) NOT NULL auto_increment,
  `page_descr` varchar(50) COLLATE utf8_general_ci NOT NULL default '',
  PRIMARY KEY  (`page_nr`),
  KEY `db_name` (`db_name`)
)
  COMMENT='PDF relation pages for phpMyAdmin'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_recent`
--

CREATE TABLE IF NOT EXISTS `pma_recent` (
  `username` varchar(64) NOT NULL,
  `tables` text NOT NULL,
  PRIMARY KEY (`username`)
)
  COMMENT='Recently accessed tables'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_uiprefs`
--

CREATE TABLE IF NOT EXISTS `pma_table_uiprefs` (
  `username` varchar(64) NOT NULL,
  `db_name` varchar(64) NOT NULL,
  `table_name` varchar(64) NOT NULL,
  `prefs` text NOT NULL,
  `last_update` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`username`,`db_name`,`table_name`)
)
  COMMENT='Tables'' UI preferences'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_relation`
--

CREATE TABLE IF NOT EXISTS `pma_relation` (
  `master_db` varchar(64) NOT NULL default '',
  `master_table` varchar(64) NOT NULL default '',
  `master_field` varchar(64) NOT NULL default '',
  `foreign_db` varchar(64) NOT NULL default '',
  `foreign_table` varchar(64) NOT NULL default '',
  `foreign_field` varchar(64) NOT NULL default '',
  PRIMARY KEY  (`master_db`,`master_table`,`master_field`),
  KEY `foreign_field` (`foreign_db`,`foreign_table`)
)
  COMMENT='Relation table'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_coords`
--

CREATE TABLE IF NOT EXISTS `pma_table_coords` (
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `pdf_page_number` int(11) NOT NULL default '0',
  `x` float NOT NULL default '0',
  `y` float NOT NULL default '0',
  PRIMARY KEY  (`db_name`,`table_name`,`pdf_page_number`)
)
  COMMENT='Table coordinates for phpMyAdmin PDF output'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_info`
--

CREATE TABLE IF NOT EXISTS `pma_table_info` (
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `display_field` varchar(64) NOT NULL default '',
  PRIMARY KEY  (`db_name`,`table_name`)
)
  COMMENT='Table information for phpMyAdmin'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_designer_coords`
--

CREATE TABLE IF NOT EXISTS `pma_designer_coords` (
  `db_name` varchar(64) NOT NULL default '',
  `table_name` varchar(64) NOT NULL default '',
  `x` INT,
  `y` INT,
  `v` INT,
  `h` INT,
  PRIMARY KEY (`db_name`,`table_name`)
)
  COMMENT='Table coordinates for Designer'
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_tracking`
--

CREATE TABLE IF NOT EXISTS `pma_tracking` (
  `db_name` varchar(64) NOT NULL,
  `table_name` varchar(64) NOT NULL,
  `version` int(10) NOT NULL,
  `date_created` datetime NOT NULL,
  `date_updated` datetime NOT NULL,
  `schema_snapshot` text NOT NULL,
  `schema_sql` text,
  `data_sql` text,
  `tracking` varchar(15) default NULL,
  `tracking_active` int(1) NOT NULL default '1',
  PRIMARY KEY  (`db_name`,`table_name`,`version`)
)
  COLLATE utf8_bin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_userconfig`
--

CREATE TABLE IF NOT EXISTS `pma_userconfig` (
  `username` varchar(64) NOT NULL,
  `timevalue` timestamp NOT NULL,
  `config_data` text NOT NULL,
  PRIMARY KEY  (`username`)
)
  COMMENT='User preferences storage for phpMyAdmin'
  COLLATE utf8_bin;
-- -------------------------------------------------------------
-- SQL Commands to upgrade pmadb for normal phpMyAdmin operation
-- with MySQL 4.1.2 and above.
--
-- This file is meant for use with MySQL 4.1.2 and above!
-- For older MySQL releases, please use create_tables.sql
--
-- If you are running one MySQL 4.1.0 or 4.1.1, please create the tables using
-- create_tables.sql, then use this script.
--
-- Please don't forget to set up the tablenames in config.inc.php
--

-- --------------------------------------------------------

--
-- Database : `phpmyadmin`
--
ALTER DATABASE `phpmyadmin`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;
USE phpmyadmin;

-- --------------------------------------------------------

--
-- Table structure for table `pma_bookmark`
--
ALTER TABLE `pma_bookmark`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_bookmark`
  CHANGE `dbase` `dbase` VARCHAR( 255 ) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_bookmark`
  CHANGE `user` `user` VARCHAR( 255 ) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_bookmark`
  CHANGE `label` `label` VARCHAR( 255 ) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '';
ALTER TABLE `pma_bookmark`
  CHANGE `query` `query` TEXT CHARACTER SET utf8 COLLATE utf8_bin NOT NULL;

-- --------------------------------------------------------

--
-- Table structure for table `pma_column_info`
--

ALTER TABLE `pma_column_info`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_column_info`
  CHANGE `db_name` `db_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_column_info`
  CHANGE `table_name` `table_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_column_info`
  CHANGE `column_name` `column_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_column_info`
  CHANGE `comment` `comment` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '';
ALTER TABLE `pma_column_info`
  CHANGE `mimetype` `mimetype` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL DEFAULT '';
ALTER TABLE `pma_column_info`
  CHANGE `transformation` `transformation` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_column_info`
  CHANGE `transformation_options` `transformation_options` VARCHAR(255) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';

-- --------------------------------------------------------

--
-- Table structure for table `pma_history`
--
ALTER TABLE `pma_history`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_history`
  CHANGE `username` `username` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_history`
  CHANGE `db` `db` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_history`
  CHANGE `table` `table` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_history`
  CHANGE `sqlquery` `sqlquery` TEXT CHARACTER SET utf8 COLLATE utf8_bin NOT NULL;

-- --------------------------------------------------------

--
-- Table structure for table `pma_pdf_pages`
--

ALTER TABLE `pma_pdf_pages`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_pdf_pages`
  CHANGE `db_name` `db_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_pdf_pages`
  CHANGE `page_descr` `page_descr` VARCHAR(50) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL default '';

-- --------------------------------------------------------

--
-- Table structure for table `pma_relation`
--
ALTER TABLE `pma_relation`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_relation`
  CHANGE `master_db` `master_db` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_relation`
  CHANGE `master_table` `master_table` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_relation`
  CHANGE `master_field` `master_field` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_relation`
  CHANGE `foreign_db` `foreign_db` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_relation`
  CHANGE `foreign_table` `foreign_table` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_relation`
  CHANGE `foreign_field` `foreign_field` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_coords`
--

ALTER TABLE `pma_table_coords`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_table_coords`
  CHANGE `db_name` `db_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_table_coords`
  CHANGE `table_name` `table_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';

-- --------------------------------------------------------

--
-- Table structure for table `pma_table_info`
--

ALTER TABLE `pma_table_info`
  DEFAULT CHARACTER SET utf8 COLLATE utf8_bin;

ALTER TABLE `pma_table_info`
  CHANGE `db_name` `db_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_table_info`
  CHANGE `table_name` `table_name` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
ALTER TABLE `pma_table_info`
  CHANGE `display_field` `display_field` VARCHAR(64) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '';
