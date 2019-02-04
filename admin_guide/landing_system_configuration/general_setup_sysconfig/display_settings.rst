.. _configuration--general-setup--display-settings:

Display Settings
================

In this section, you can define a number of display-related options to be applied to the Oro application. The settings are available on four levels: globally, per organization, per website, and per user.

.. note:: See a short demo on how to set display settings in your Oro application, or continue reading the guidance below.

   .. raw:: html

      <iframe width="560" height="315" src="https://www.youtube.com/embed/B2DqoTVQCao" frameborder="0" allowfullscreen></iframe>

.. contents::
   :local:

Configure Display Settings Globally
-----------------------------------

To open display settings:

1. Navigate to **System > Configuration** in the main menu.
2. Select **System Configuration > General Setup > Display Settings** in the menu to the left.

.. image:: /admin_guide/img/configuration/display_settings.png
   :width: 500

**User Bar**

+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field              | Description                                                                                                                                           |
+====================+=======================================================================================================================================================+
| Show Recent Emails | Select this check box to display the recent emails on the user bar (they will appear next to the user name). The functionality is enabled by default. |
|                    |                                                                                                                                                       |
|                    | .. image:: /user_guide/img/admin/user_management/user_configuration_showemailsuserbar.png                                                             |
|                    |    :alt: A recent emails icon displayed on the user bar                                                                                               |
|                    |                                                                                                                                                       |
+--------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------+


**Navigation bar**

+----------+----------------------------------------------------------------------------------------------------+
| Field    | Description                                                                                        |
+==========+====================================================================================================+
| Position | Select whether the OroCommerce main menu will be positioned at the top of the page or on its left. |
+----------+----------------------------------------------------------------------------------------------------+

.. _doc-configuration-display-settings:

**Data Grid Settings**

Data grid settings define different options used to display all the record lists (grids) in the management console.

The following options are available:

+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Field                     | Description                                                                                                    |
+===========================+================================================================================================================+
| Items Per Page By Default | Defines the number of items displayed on one page of the grid by default (every time you open the grid).       |
+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Lock Headers In Grids     | Select this check box to ensure that headers of a record grid will stay visible while you scroll.              |
+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Record Pagination         | Select this check box to enable the user navigate to the previous or next grid record from a record view page. |
|                           |                                                                                                                |
|                           | .. image:: /user_guide/img/admin/user_management/user_configuration_pagination.png                             |
|                           |    :alt: A record pagination sample                                                                            |
|                           |                                                                                                                |
+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Record Pagination Limit   | Defines a maximum number of records available for the record pagination                                        |
+---------------------------+----------------------------------------------------------------------------------------------------------------+


**Activity Lists**

The activity list setting defines different options to be applied to display :ref:`activities <user-guide-activities>`.

The following options are available:

+---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
| Field                     | Description                                                                                                                         |
+===========================+=====================================================================================================================================+
| Sort By Field             | Select whether to sort activity records by the date when they were created or by the date when they were updated for the last time. |
+---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
| Sort Direction            | Select whether to sort records in the ascending or descending direction.                                                            |
+---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+
| Items Per Page By Default | Select how many records will appear on one page of the activity grids.                                                              |
+---------------------------+-------------------------------------------------------------------------------------------------------------------------------------+

**WYSIWYG Settings**

Define whether text formatting tools must be available for emails, notes, and comments.

The value is enabled by default.

+-----------------------+-----------------------------------------------------------------------------------------------------------+
| Field                 | Description                                                                                               |
+=======================+===========================================================================================================+
| Enable WYSIWYG Editor | Select this check box to enable text formatting tools for emails, notes and comments.                     |
|                       |                                                                                                           |
|                       | .. image:: /admin_guide/img/user_management/user_configuration_wysiwyg.png                                |
|                       |    :alt: A formatting tool bar that enables editing a text for emails, notes, and comments                |
|                       |                                                                                                           |
+-----------------------+-----------------------------------------------------------------------------------------------------------+

.. note::

    The formatting tools can also be enabled for other text fields in the course of integration.

**Sidebar Settings**

By adjusting the sidebar settings you can enable or disable the left and/or right sidebar to keep your sticky notes
and task lists.

+----------------------+-------------------------------------------------------------------------+
| Field                | Description                                                             |
+======================+=========================================================================+
| Enable Left Sidebar  | Select **Yes** to enable the user to see and utilize the left sidebar.  |
+----------------------+-------------------------------------------------------------------------+
| Enable Right Sidebar | Select **Yes** to enable the user to see and utilize the right sidebar. |
+----------------------+-------------------------------------------------------------------------+

By default, only the right sidebar is enabled.

**Tag Settings**

Tag settings specify the taxonomy colors available in the system.

.. image:: /admin_guide/img/configuration/tag_settings.png


**Calendar Settings**

Calendar settings specify the colors available to manage calendars:

+----------------------+------------------------------------------------------------------------------+
| Field                | Description                                                                  |
+======================+==============================================================================+
| Calendar Colors      | A set of colors available for color coding different organization calendars. |
+----------------------+------------------------------------------------------------------------------+
| Event Colors         | A set of colors available for color coding different organization event.     |
+----------------------+------------------------------------------------------------------------------+

To change any color in the set:

1. Click it. The color picker opens.
2. Drag and drop a dot on the color picker wheel to select a new color.
3. Adjust the color brightness by dragging the level on the shades bar.

.. image:: /admin_guide/img/configuration/calendar_settings.png

**Map Settings**

+----------------------+-------------------------------------------------------------------------------------------------+
| Field                | Description                                                                                     |
+======================+=================================================================================================+
| Enable Map Preview   | Select whether to show the location on a map when a customer views an address in the storefront.|
|                      | This option does not affect maps in the management console.                                     |
+----------------------+-------------------------------------------------------------------------------------------------+

.. image:: /admin_guide/img/configuration/map_settings_map.png

**Reports Settings**

+-------------------------------------+------------------------------------------------------------------------------------------------------------------+
| Field                               | Description                                                                                                      |
+=====================================+==================================================================================================================+
| Display SQL In Reports And Segments | Select this check box to enable the user to review the SQL request sent to the system for a report or a segment. |
|                                     | This way, users can check if a report has been developed correctly.                                              |
+-------------------------------------+------------------------------------------------------------------------------------------------------------------+

.. image:: /user_guide/img/admin/user_management/user_configuration_showsql.png
   :alt: A sample of the enabled display SQL field

.. hint::

    This link will only be available if the View SQL query of a report/segment capability has been enabled for the role.

.. _configuration--general-setup--display-settings--organization:

Configure Display Settings per Organization
-------------------------------------------

.. include:: /admin_guide/config_levels/org/general_setup_sysconfig/organization_display_settings.rst
   :start-after: begin_display_set_org
   :end-before: finish_display_set_org

.. _doc-my-user-configuration-display:

Configure Display Settings per User
-----------------------------------

.. include:: /admin_guide/config_levels/user.rst
   :start-after: begin_display_set_user
   :end-before: finish_display_set_user


.. include:: /img/buttons/include_images.rst
   :start-after: begin