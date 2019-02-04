.. _web-api--configuration:

Configuration Reference
=======================

.. contents:: :local:

Overview
--------

The configuration declares all aspects related to specific entity. The configuration should be placed in ``Resources/config/oro/api.yml`` to be automatically loaded.

All entities, except custom entities, dictionaries and enumerations are not accessible through Data API. To allow usage of an entity in Data API you have to enable it directly. For example, to make ``Acme\Bundle\ProductBundle\Product`` entity available through Data API you can write the following configuration:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\ProductBundle\Product: ~

If an auto-generated alias for your entity looks bad for you, you can change it in ``Resources/config/oro/entity.yml``. More details you can find in `entity aliases documentation <https://github.com/oroinc/platform/tree/master/src/Oro/Bundle/EntityBundle/Resources/doc/entity_aliases.md>`__.

Do not forget to run ``oro:api:cache:clear`` CLI command to immediately make an entity accessible through Data API. If you use API sandbox run ``oro:api:doc:cache:clear`` CLI command to apply the changes for it. Also please see other :ref:`CLI commands <web-api--commands>` that may be helpful.


Configuration Structure
-----------------------

To get the overall configuration structure, execute the following command:

.. code:: bash

    php bin/console oro:api:config:dump-reference

By default this command shows configuration of nesting entities. To simplify the output you can use the ``--max-nesting-level`` option, e.g.

.. code:: bash

    php bin/console oro:api:config:dump-reference --max-nesting-level=0

The default nesting level is ``3``. It is specified in the configuration of ApiBundle via the ``config_max_nesting_level`` parameter. So, if needed, you can easily change this value, for example:

.. code:: yaml

    # config/config.yml

    oro_api:
        config_max_nesting_level: 3

The first level sections of configuration are:

-  `entity\_aliases <#entity_aliases-configuration-section>`__ - allows to override entity aliases.
-  `entities <#entities-configuration-section>`__ - describes the configuration of entities.
-  `relations <#relations-configuration-section>`__ - describes the configuration of relationships.

Top level configuration example:

.. code:: yaml

    api:
        entity_aliases:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                ...

        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                exclude:
                ...
                fields:
                    ...
                filters:
                    fields:
                        ...
                sorters:
                    fields:
                        ...
                actions:
                    ...
                subresources:
                    ...
            ...
        relations:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                ...
                fields:
                    ...
                filters:
                    fields:
                        ...
                sorters:
                    fields:
                        ...
            ...


"exclude" option
----------------

The ``exclude`` configuration option describes whether an entity or some of its fields should be excluded from Data API.

Example:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity1:
                exclude: true # exclude the entity from Data API
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity2:
                fields:
                    field1:
                        exclude: true # exclude the field from Data API

Also the ``exclude`` option can be used to indicate whether filtering or sorting for certain field should be disabled. Please note that filtering and sorting for the excluded field are disabled automatically, so it's not possible to filter or sort by excluded field.

Example:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity1:
                sorters:
                    fields:
                        field1:
                            exclude: true
                filters:
                    fields:
                        field1:
                            exclude: true

Please note that ``exclude`` option are applicable only for Data API. In case if an entity or its' field(s) should be excluded globally use ``Resources/config/oro/entity.yml``, e.g.:

.. code:: yaml

    oro_entity:
        exclusions:
            # whole entity exclusion
            - { entity: Acme\Bundle\AcmeBundle\Entity\AcmeEntity1 }
            # exclude field1 of Acme\Bundle\AcmeBundle\Entity\Entity2 entity
            - { entity: Acme\Bundle\AcmeBundle\Entity\AcmeEntity2, field: field1 }

"entity\_aliases" configuration section
---------------------------------------

The ``entity_aliases`` section allows to override existing system-wide entity aliases or add aliases for models intended to be used only in Data API.

It can be helpful when you need to provide entity aliases for Data API but it is not possible to make them system-wide. For example because the backwards compatibility promise or because your models were created for using only in Data API.

Please see `documentation <https://github.com/oroinc/platform/tree/master/src/Oro/Bundle/EntityBundle/Resources/doc/entity_aliases.md>`__ for more details about entity aliases.

An example:

.. code:: yaml

    api:
        entity_aliases:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                alias: acmeentity
                plural_alias: acmeentities

"entities" configuration section
--------------------------------

The ``entities`` section describes a configuration of entities.

-  **documentation\_resource** *string* May contain the link to `markdown <https://en.wikipedia.org/wiki/Markdown>`__ file that contains a detailed documentation for a single or multiple API resources. For more details see :ref:`Documenting API Resources <web-api--doc>`. Please note that the same entity can be configured in different ``Resources/config/oro/api.yml`` files, e.g. when some bundle needs to add a field to an entity declared in another bundle. In this case all configuration files
   for this entity can have **documentation\_resource** option and all documentation files declared there will be merged. Also pay attention that in case if the same field is documented in several documentation files, they will not be merged and only a documentation from one file will be used.

-  **exclude** *boolean* Indicates whether the entity should be excluded from Data API. By default ``false``.
-  **inherit** *boolean* By default ``true``. The flag indicates that the configuration for certain entity should be merged with the configuration of a parent entity. If a derived entity should have completely different configuration and merging with parent configuration is not needed the flag should be set to ``false``.
-  **exclusion\_policy** *string* - Can be ``all`` or ``none``. By default ``none``. Indicates the exclusion strategy that should be used for the entity. ``all`` means that all fields are not configured explicitly will be excluded. ``none`` means that only fields marked with ``exclude`` flag will be excluded.
-  **max\_results** *integer* The maximum number of entities in the result. Set ``-1`` (it means unlimited), zero or positive number to set the limit. Can be used to set the limit for both root and related entities.
-  **order\_by** *array* The property can be used to configure default ordering of the result. The item key is the name of a field. The value can be ``ASC`` or ``DESC``. By default the result is ordered by identifier field.
-  **disable\_inclusion** *boolean* The flag indicates whether an inclusion of related entities is disabled. In JSON.API an `**include** request parameter <http://jsonapi.org/format/#fetching-includes>`__ can be used to customize which related entities should be returned. By default ``false``.
-  **disable\_fieldset** *boolean* The flag indicates whether a requesting of a restricted set of fields is disabled. In JSON.API an `**fields** request parameter <http://jsonapi.org/format/#fetching-sparse-fieldsets>`__ can be used to customize which fields should be returned. By default ``false``.
-  **disable\_meta\_properties** *boolean* The flag indicates whether a requesting of additional meta properties is disabled. By default ``false``.
-  **hints** *array* Sets `Doctrine query hints <http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/dql-doctrine-query-language.html#query-hints>`__. Each item can be a string or an array with ``name`` and ``value`` keys. The string value is a short form of ``[name: hint name]``.
-  **identifier\_field\_names** *string[]* The names of identifier fields of the entity. Usually it should be set in a configuration file in case if Data API resource is not based on ORM entity. For ORM entities a value of this option is retrieved from an entity metadata, but this can be changed using this option if by some reasons you do not want to use the primary key as an entity identifier in Data API.
-  **delete\_handler** *string* The id of a service that should be used to delete entity by the :ref:`delete <web-api--actions>` and :ref:`delete\_list <web-api--actions>` actions. By default the `oro\_soap.handler.delete <https://github.com/oroinc/platform/tree/master/src/Oro/Bundle/SoapBundle/Handler/DeleteHandler.php>`__ service is used.
-  **form\_type** *string* The form type that should be used for the entity in :ref:`create <web-api--actions>` and :ref:`update <web-api--actions>` actions. By default the ``form`` form type is used.
-  **form\_options** *array* The form options that should be used for the entity in :ref:`create <web-api--actions>` and :ref:`update <web-api--actions>` actions.
-  **form\_event\_subscriber** The form event subscriber(s) that should be used for the entity in :ref:`create <web-api--actions>` and :ref:`update <web-api--actions>` actions. Also this event subscriber is used for :ref:`update\_relationship <web-api--actions>`, :ref:`add\_relationship <web-api--actions>` and :ref:`delete\_relationship <web-api--actions>` actions, but only if the ``form_type`` option is not specified. For custom form types this event subscriber is not used. Can be specified as service name or array of service names. An event subscriber service should implement ``Symfony\Component\EventDispatcher\EventSubscriberInterface`` interface.

By default the following form options are set:

+--------------------------+--------------------------------------------------------------------+
| Option Name              | Option Value                                                       |
+==========================+====================================================================+
| data\_class              | The class name of the entity                                       |
+--------------------------+--------------------------------------------------------------------+
| validation\_groups       | ['Default', 'api']                                                 |
+--------------------------+--------------------------------------------------------------------+
| extra\_fields\_message   | This form should not contain extra fields: {{ extra\_fields }}.    |
+--------------------------+--------------------------------------------------------------------+

Example:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                documentation_resource: '@AcmeAcmeBundle/Resources/doc/api/acme_entity.md'
                inherit:              false
                exclusion_policy:     all
                max_results:          25
                order_by:
                    field1: DESC
                    field2: ASC
                hints:
                    - HINT_TRANSLATABLE
                    - { name: HINT_FILTER_BY_CURRENT_USER }
                    - { name: HINT_CUSTOM_OUTPUT_WALKER, value: 'Acme\Bundle\AcmeBundle\AST_Walker_Class'}
                delete_handler:       acme.demo.test_entity.delete_handler
                excluded:             false
                form_type: acme_entity.api_form
                form_options:
                    validation_groups: ['Default', 'api', 'my_group']
                form_event_subscriber: acme.api.form_listener.test_entity


"fields" configuration section
------------------------------

This section describes entity fields' configuration.

-  **exclude** *boolean* Indicates whether the field should be excluded. This property is described above in `"exclude" option <#exclude-option>`__.
-  **description** *string* A human-readable description of the field or a link to the :ref:`documentation resource <web-api--doc>`. Used in auto-generated documentation only.
-  **property\_path** *string* The property path to reach the fields' value. Can be used to rename a field or to access to a field of related entity.
-  **collapse** *boolean* Indicates whether the entity should be collapsed. It is applicable for associations only. It means that target entity should be returned as a value, instead of an array with values of entity fields. Usually this property is set by *get\_relation\_config* processors to get identifier of the related entity.
-  **form\_type** *string* The form type that should be used for the field in *create* and *update* actions.
-  **form\_options** *array* The form options that should be used for the field in *create* and *update* actions.
-  **data\_type** *string* The data type of the field value. Can be ``boolean``, ``integer``, ``string``, etc. If a field represents an association the data type should be a type of an identity field of the target entity.
-  **meta\_property** *boolean* A flag indicates whether the field represents a meta information. For JSON.API such fields will be returned in `meta <http://jsonapi.org/format/#document-meta>`__ section. By default ``false``.
-  **target\_class** *string* The class name of a target entity if a field represents an association. Usually it should be set in a configuration file in case if Data API resource is based on not ORM entity.
-  **target\_type** *string* The type of a target association. Can be **to-one** or **to-many**. Also **collection** can be used as an alias for **to-many**. **to-one** can be omitted as it is used by default. Usually it should be set in a configuration file in case if Data API resource is based on not ORM entity.
-  **depends\_on** *string[]* A list of fields on which this field depends on. Also ``.`` can be used to specify a path to an association field. This option can be helpful for computed fields. These fields will be loaded from the database even if they are excluded.

Special data types:

As described above, the **data\_type** attribute can be used to specify a data type of a field, but it can be used to configure some special types of fields as well. The following table contains details of such types.

+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Data Type                                    | Description                                                                                                                                                                                                                                                                                                                                                                                                  |
+==============================================+==============================================================================================================================================================================================================================================================================================================================================================================================================+
| scalar                                       | Used to represent a field of to-one association as a field of parent entity. In JSON.API it means that the association's field should be in "attributes" section instead of "relationships" section.                                                                                                                                                                                                         |
+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| object                                       | Used to represent to-one association as a field. In JSON.API it means that the association should be in "attributes" section instead of "relationships" section.                                                                                                                                                                                                                                             |
+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| array                                        | Used to represent to-many association as a field. In JSON.API it means that the association should be in "attributes" section instead of "relationships" section.                                                                                                                                                                                                                                            |
+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| nestedObject                                 | Used to configure nested objects. For details see :ref:`Configure nested object <web-api--how-to>`.             .                                                                                                                                                                                                                                                                                            |
+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| nestedAssociation                            | Used to configure nested associations. For details see :ref:`Configure nested association <web-api--how-to>`.                                                                                                                                                                                                                                                                                                |
+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| association:relationType[:associationKind]   | Used to configure extended associations. For details see the :ref:`How to <web-api--how-to>` topic.                                                                                                                                                                                                                                                                                                          |
+----------------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                fields:
                    # exclude a field
                    field1:
                        exclude: true

                    # the "firstName" field will be renamed to the "name" field
                    name:
                        description: Some Field
                        property_path: firstName

                    # the "addressName" field will contain the value of the "name" field of the "address" related entity
                    addressName:
                        property_path: address.name

                    # full syntax for "collapse" property
                    field4:
                        collapse:         true
                        exclusion_policy: all
                        fields:
                            targetField1: null

                    # short syntax for "collapse" property
                    field5:
                        fields: targetField1

                    # form type and form options for a field
                    field6:
                        form_type: text
                        form_options:
                            trim: false
                            constraints:
                                # add Symfony\Component\Validator\Constraints\NotBlank validation constraint
                                - NotBlank: ~

                    # to-one association
                    field7:
                        data_type: integer # the data type of an identifier field of the target
                        target_class: Acme\Bundle\AcmeBundle\Api\Model\AcmeTargetEntity

                    # to-many association
                    field8:
                        data_type: integer # the data type of an identifier field of the target
                        target_class: Acme\Bundle\AcmeBundle\Api\Model\AcmeTargetEntity
                        target_type: collection

                    # computed field
                    field9:
                        data_type: string
                        depends_on: [field1, association1.field11]

"filters" configuration section
-------------------------------

This section describes fields by which the result data can be filtered. It contains two properties: ``exclusion_policy`` and ``fields``.

-  **exclusion\_policy** *string* Can be ``all`` or ``none``. By default ``none``. Indicates the exclusion strategy that should be used. ``all`` means that all fields are not configured explicitly will be excluded. ``none`` means that only fields marked with ``exclude`` flag will be excluded.
-  **fields** This section describes a configuration of each field that can be used to filter the result data. Each filter can have the following properties:

   -  **exclude** *boolean* Indicates whether filtering by this field should be disabled. By default ``false``.
   -  **description** *string* A human-readable description of the filter or a link to the :ref:`documentation resource <web-api--doc>`. Used in auto-generated documentation only.
   -  **property\_path** *string* The property path to reach the fields' value. The same way as above in ``fields`` configuration section.
   -  **data\_type** *string* The data type of the filter value. Can be ``boolean``, ``integer``, ``string``, etc.
   -  **allow\_array** *boolean* A flag indicates whether the filter can contain several values. By default ``false``.
   -  **allow\_range** *boolean* A flag indicates whether the filter can contain a pair of "from" and "to" values. By default ``false``.
   -  **type** *string* The filter type. By default the filter type is equal to the **data\_type** property.
   -  **options** *array* The filter options.
   -  **operators** *array* A list of operators supported by the filter. By default the list of operators depends on the filter type. For example a string filter supports **=** and **!=** operators, a number filter supports **=**, **!=**, **<**, **<=**, **>** and **>=** operators, etc. Usually you need to use this parameter in case if you need to make a list of supported operators more limited.

Example:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                filters:
                    exclusion_policy: all
                    fields:
                        field1:
                            data_type: integer
                            exclude: true
                        field2:
                            data_type: string
                            property_path: firstName
                            description: 'My filter description'
                        field3:
                            data_type: date
                            allow_array: true
                            allow_range: true
                        field4:
                            data_type: string
                            type: myFilter
                            options:
                                my_option: value
                        field5:
                            operators: ['=']

"sorters" configuration section
-------------------------------

This section describes fields by which the result data can be sorted. It contains two properties: ``exclusion_policy`` and ``fields``.

-  **exclusion\_policy** *string* Can be ``all`` or ``none``. By default ``none``. Indicates the exclusion strategy that should be used. ``all`` means that all fields are not configured explicitly will be excluded. ``none`` means that only fields marked with ``exclude`` flag will be excluded.
-  **fields** - This section describes a configuration of each field that can be used to sort the result data. Each sorter can have the following properties:

   -  **exclude** *boolean* Indicates whether sorting by this field should be disabled. By default ``false``.
   -  **property\_path** *string* The property path to reach the fields' value. The same way as above in ``fields`` configuration section.

Example:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                sorters:
                    fields:
                        field1:
                            property_path: firstName
                        field2:
                            exclude: true

"actions" configuration section
-------------------------------

The ``actions`` configuration section allows to specify action-specific options. The options from this section will be added to the entity configuration. If an option exists in both entity and action configurations the action option wins. The exception is the ``exclude`` option. This option is used to disable an action for a specific entity and it is not copied to the entity configuration.

-  **exclude** *boolean* Indicates whether the action is disabled for entity. By default ``false``.
-  **description** *string* A short, human-readable description of API resource. Used in auto-generated documentation only.
-  **documentation** *string* A detailed documentation of API resource or a link to the :ref:`documentation resource <web-api--doc>`. Used in auto-generated documentation only.
-  **acl\_resource** *string* The name of ACL resource that should be used to protect an entity in a scope of this action. The ``null`` can be used to disable access checks.
-  **max\_results** *integer* The maximum number of entities in the result. Set ``-1`` (it means unlimited), zero or positive number to set the limit. Can be used to set the limit for both root and related entities.
-  **order\_by** *array* The property can be used to configure default ordering of the result. The item key is the name of a field. The value can be ``ASC`` or ``DESC``. By default the result is ordered by identifier field.
-  **page\_size** *integer* The default page size. Set a positive number or ``-1`` if a pagination should be disabled. Default value is ``10``.
-  **disable\_sorting** *boolean* The flag indicates whether a sorting is disabled. By default ``false``.
-  **disable\_inclusion** *boolean* The flag indicates whether an inclusion of related entities is disabled. In JSON.API an `**include** request parameter <http://jsonapi.org/format/#fetching-includes>`__ can be used to customize which related entities should be returned. By default ``false``.
-  **disable\_fieldset** *boolean* The flag indicates whether a requesting of a restricted set of fields is disabled. In JSON.API an `**fields** request parameter <http://jsonapi.org/format/#fetching-sparse-fieldsets>`__ can be used to customize which fields should be returned. By default ``false``.
-  **disable\_meta\_properties** *boolean* The flag indicates whether a requesting of additional meta properties is disabled. By default ``false``.
-  **form\_type** *string* The form type that should be used for the entity.
-  **form\_options** *array* The form options that should be used for the entity. Please note that these form options are merged with form options are defined on the entity level, but only in case if the ``form_type`` is not specified. If ``form_type`` is specified in an action configuration the action form options completely replace the form options are defined on the entity level.
-  **form\_event\_subscriber** The form event subscriber(s) that should be used for the entity. Can be specified as service name or array of service names. An event subscriber service should implement ``Symfony\Component\EventDispatcher\EventSubscriberInterface`` interface. Please note that these event subscribers are merged with event subscribers are defined on the entity level, but only in case if the ``form_type`` is not specified. If ``form_type`` is specified in an action configuration the
   action event subscribers completely replace the event subscribers are defined on the entity level.
-  **status\_codes** *array* The possible response status codes for the action.

   -  **exclude** *boolean* Indicates whether the status code should be excluded for a particular action. This property is described above in `"exclude" option <#exclude-option>`__.
   -  **description** *string* A human-readable description of the status code. Used in auto-generated documentation only.

-  **fields** - This section describes entity fields' configuration specific for a particular action.

   -  **exclude** *boolean* Indicates whether the field should be excluded for a particular action. This property is described above in `"exclude" option <#exclude-option>`__.
   -  **form\_type** *string* The form type that should be used for the field.
   -  **form\_options** *array* The form options that should be used for the field.

By default, the following permissions are used to restrict access to an entity in a scope of the specific action:

+----------------+-------------------+
| Action         | Permission        |
+================+===================+
| get            | VIEW              |
+----------------+-------------------+
| get\_list      | VIEW              |
+----------------+-------------------+
| create         | CREATE and VIEW   |
+----------------+-------------------+
| update         | EDIT and VIEW     |
+----------------+-------------------+
| delete         | DELETE            |
+----------------+-------------------+
| delete\_list   | DELETE            |
+----------------+-------------------+

Examples of ``actions`` section configuration:

Disable all action for an entity:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                # this entity does not have own Data API resource
                actions: false

Disable ``delete`` action for an entity:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    delete:
                        exclude: true

Also a short syntax can be used:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    delete: false

Set custom ACL resource for the ``get_list`` action:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    get_list:
                        acl_resource: acme_view_resource

Turn off access checks for the ``get`` action:

.. code:: yaml

    api:
        entities:
           Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    get:
                        acl_resource: ~

Add additional status code for ``delete`` action:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    delete:
                        status_codes:
                            '417': 'Returned when expectations failed'

or

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    delete:
                        status_codes:
                            '417':
                                description: 'Returned when expectations failed'

Remove existing status code for ``delete`` action:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    delete:
                        status_codes:
                            '417': false

or

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    delete:
                        status_codes:
                            '417':
                                exclude: true

Exclude a field for ``update`` action:

.. code:: yaml

    api:
        entities:
            Acme\Bundle\AcmeBundle\Entity\AcmeEntity:
                actions:
                    update:
                        fields:
                            field1:
                                exclude: true

"subresources" configuration section
------------------------------------

The ``subresources`` configuration section allows to provide options for sub-resources.

-  **exclude** *boolean* Indicates whether the sub-resource is disabled for entity. By default ``false``.
-  **target\_class** *string* The class name of a target entity.
-  **target\_type** *string* The type of a target association. Can be **to-one** or **to-many**. Also **collection** can be used as an alias for **to-many**. **to-one** can be omitted as it is used by default.
-  **actions** *array* The actions supported by the sub-resource. This section has the same options as `entity **actions** section <#actions-configuration-section>`__. If an option exists in both `entity **actions** section <#actions-configuration-section>`__ and sub-resource **actions** section the sub-resource option wins.
-  **filters** - The filters supported by the sub-resource. This section has the same options as `entity **filters** section <#filters-configuration-section>`__. If an option exists in both `entity **filters** section <#filters-configuration-section>`__ and sub-resource **filters** section the sub-resource option wins.

Example:

.. code:: yaml

    api:
        entities:
            Oro\Bundle\EmailBundle\Entity\Email:
                subresources:
                    suggestions:
                        target_class: Oro\Bundle\ApiBundle\Model\EntityIdentifier
                        target_type: collection
                        actions:
                            get_subresource:
                                description: Get entities that might be associated with the email
                            get_relationship: false
                            update_relationship: false
                            add_relationship: false
                            delete_relationship: false
                        filters:
                            fields:
                                exclude-current-user:
                                    description: Indicates whether the current user should be excluded from the result.
                                    data_type: boolean

"relations" configuration section
---------------------------------

The ``relations`` configuration section describes a configuration of an entity if it is used in a relationship. This section is not used for JSON.API, but can be helpful for other types of API. This section is similar to the `entities <#entities-configuration-section>`__ section.
