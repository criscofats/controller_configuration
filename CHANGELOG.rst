============================================
infra.controller_configuration Release Notes
============================================

.. contents:: Topics


v2.3.1
======

Bugfixes
--------

- Ensures vars get loaded properly by dispatch role
- Fixed issue in filetree_read where arg spec incorrect and caused failure (#550)

v2.3.0
======

Minor Changes
-------------

- Adapt filetree_read role tests playbook config-controller-filetree.yml.
- Add new type of objects for object_diff role:  applications, execution environments, instance groups, notifications and schedules
- Add no_log to all tasks that populates data to avoid exposing encrypted data
- Add task to add Galaxy credentials and Execution Environments to Organization.
- Set the variables to assign_galaxy_credentials_to_org and assign_default_ee_to_org to false in the task to run all roles at dispatch role.
- avoid to create orgs during drop_diff
- fixed an extra blank line in schedules readme that was breaking the table
- removed references to redhat_cop as a collection namespace in the readme files.

Breaking Changes / Porting Guide
--------------------------------

- updated object_diff role to use the infra namespace, that means to use the role it requires the infra version of the collection. Previous version required the redhat_cop

Bugfixes
--------

- Added argument_spec for all roles
- Fixed name of task for inventory source update
- Fixed variable definitions in readmes
- Removed master_role_example as no longer required (this wasn't a functional role)

v2.2.5
======

Minor Changes
-------------

- Add max_forks, max_concurrent_jobs as options to instance_groups role
- Add no_log everywhere controller_api_plugin is used to avoid to expose sensitive information in case of crashes.
- Add no_log everywhere controller_api_plugin is used to avoid to expose sensitive information in case of crashes.
- Add or fix some variables or extra_vars exported from objects like notifications, inventory, inventory_source, hosts, groups, jt or wjt.
- Add roles object to object_diff role and controller_object_diff lookup plugin.
- Fix one query with controller_password to change it and set oauth_token=controller_oauthtoken.
- Fixed typos in README.md.
- Improve template to export settings with filetree_create role. Settings will be in yaml format.
- Renamed the field `update` to `update_project` to avoid colliding with the Python dict update method
- Renamed variable controller_workflow_job_templates to controller_workflows (the previos one was not used at all).
- Renamed variable controller_workflow_job_templates to controller_workflows (the previos one was not used at all).
- return_all: true has been added to return the maximum of max_objects=query_controller_api_max_objects objects.

Bugfixes
--------

- Enable the ability to define simple_workflow_nodes on workflow_job_templates without the need to set the `state` on a workflow_job_template (https://github.com/redhat-cop/controller_configuration/issues/297).

v2.2.4
======

Minor Changes
-------------

- Update release process to avoid problems that have happened and automate it.
- removed all examples from repo outside of readmes

Breaking Changes / Porting Guide
--------------------------------

- infra.controller_configuration 2.2.3 is broken, it is aap_utilities release. We are bumping the version to minimize the issues.
- rewrote playbooks/controller_configure.yml and removed all other playbooks

Removed Features (previously deprecated)
----------------------------------------

- update_on_project_update in inventory_source as an option due to the awx module no longer supports this option.

v2.1.9
======

Major Changes
-------------

- Added instance role to add instances using the new awx.awx.instance module.

Minor Changes
-------------

- Update options on inventories, job templates, liscence, projects, schedules, and workflow_job_templates roles to match latest awx.awx release

v2.1.8
======

Minor Changes
-------------

- Add a way to detect which of `awx.awx` or `ansible.controller` collection is installed. Added to the playbooks and examples.
- Add markdown linter
- Add the current object ID to the corresponding output yaml filename.
- Fix all linter reported errors
- Move linter configurations to root directory
- Organize the output in directories (one per each object type).
- Remove json_query and jmespath dependency from filetree_create role.
- Update linter versions

Bugfixes
--------

- Fixed optional lists to default to omit if the list is empty.
- Reduce the memory usage on the filetree_create role.

v2.1.7
======

Major Changes
-------------

- Adds Configuration as Code filetree_create - A role to export and convert all  Controller's objects configuration in yaml files to be consumed with previous roles.
- Adds Configuration as Code filetree_read role - A role to load controller variables (objects) from a hierarchical and scalable directory structure.
- Adds Configuration as Code object_diff role - A role to get differences between code and controller. It will give us the lists to remove absent objects in the controller which they are not in code.

Minor Changes
-------------

- Adds credential and organization options for schedule role.
- inventory_sources - update ``source_vars`` to parse Jinja variables using the same workaround as inventories role.

v2.1.6
======

Bugfixes
--------

- Fixed broken documentation for controller_object_diff plugin

v2.1.5
======

Major Changes
-------------

- Adds dispatch role - A role to run all other roles.

Bugfixes
--------

- Changed default interval for inventory_source_update, project_update and project to be the value of the role's async delay value. This still defaults to 1 if the delay value is not set as previously.

v2.1.4
======

Bugfixes
--------

- Fixes async to work on default execution enviroments.
- Fixes inventories hardcoded 'no_log' true on the async job check task.

v2.1.3
======

Minor Changes
-------------

- Added asynchronous to {organizations,credentials,credential_types,inventories,job_templates} task to speed up creation.
- Allow setting the organization when creating users.
- Update to controller_object_diff lookup plugin to better handle group, host, inventory, credential, workflow_job_template_node and user objects.
- Update to controller_object_diff lookup plugin to better handle organizations.

Breaking Changes / Porting Guide
--------------------------------

- galaxy credentials in the organization role now require assign_galaxy_organizations_to_org to be true.

Bugfixes
--------

- Fixes option of `survey_spec` on job_templates role.

v2.1.1
======

Minor Changes
-------------

- Allows for using the roles for deletion to only use required fields.
- Changed default to omit for several fields for notification templates and inventor sources.
- These changes are in line with the modules required fields.

Bugfixes
--------

- warn on default if the api list fed to controller_object_diff lookup is empty

v2.1.0
======

Major Changes
-------------

- added diff plugin and tests for diff plugin to aid in removal tasks

Minor Changes
-------------

- Added new options for adding manifest to Ansible Controller inc. from a URL and from b64 encoded content
- added tests for the project and inventory source skips

Bugfixes
--------

- Fixed readme's to point in right direction for workflows and the export model in examples
- Moved Example playbooks to the example directory
- Removes json_query which is not in a RH Certified collection so does not receive support and replaced with native ansible filters
- Updated workflow inventory option to be able to use workflows from the export model.
- added default to organization as null on project as it is not required for the module, but it is highly recommended.
- added when to skip inventory source update when item is absent
- added when to skip project update when item is absent

v2.0.0
======

Major Changes
-------------

- Created awx and controller playbook that users can invoke for using the collection

Minor Changes
-------------

- Additional module options have been added such as instance_groups and copy_from where applicable.
- All role tests have been converted to use one format.
- Created Readme for playbook in the playbooks directory
- Removed the playbook configs folder, it was previously moved to the .github/playbooks directory

Breaking Changes / Porting Guide
--------------------------------

- All references to tower have been changed to Controller.
- Changed all module names to be in line with changes to awx.awx as of 19.2.1.
- Changed variable names for all objects from tower_* to controller_*.
- Removed depreciated module options for notification Templates.

Bugfixes
--------

- Changed all references for ansible.tower to ansible.controller
- Fixed issue where `credential` was not working for project and instead the old `scm_credential` option remained.

v1.5.0
======

Major Changes
-------------

- Removed testing via playbook install that was removed in awx 18.0.0.
- Updated testing via playbook to use minikube + operator install.

Breaking Changes / Porting Guide
--------------------------------

- Examples can also be found in the playbooks/tower_configs_export_model/tower_workflows.yml
- If you do not change the data model, change the variable 'workflow_nodes' to 'simplified_workflow_nodes'.
- More information can be found either in the Workflow Job Template Readme or on the awx.awx.tower_workflow_job_template Documentation.
- The Tower export model is now the default to use under workflow nodes. This is documented in the workflow job templates Readme.
- Users using the tower export model previously, do not need to make any changes.
- Workflow Schemas to describe Workflow nodes have changed.

Bugfixes
--------

- Allow tower_hostname and tower_validate_certs to not be set in favour of environment variables being set as per module defaults.
- Changes all boolean variables to have their default values omitted rather than using the value 'default(omit, true)' which prevents a falsy value being supplied.

v1.4.1
======

Major Changes
-------------

- Added execution environments option for multiple roles.
- Added execution environments role.

Bugfixes
--------

- Fix tower_templates default

v1.3.0
======

Bugfixes
--------

- Fixed an issue where certain roles were not taking in tower_validate_certs

v1.2.0
======

Breaking Changes / Porting Guide
--------------------------------

- removed awx.awx implicit dependency, it will now be required to manually install awx.awx or ansible.tower collection

v1.1.0
======

Major Changes
-------------

- Added the following roles - ad_hoc_command, ad_hoc_command_cancel, inventory_source_update, job_launch, job_cancel, project_update, workflow_launch
- Updated collection to use and comply with ansible-lint v5

Minor Changes
-------------

- Fixed default filters to use true when neccessary and changed a few defaults to omit rather then a value or empty string.
- updated various Readmes to fix typos and missing information.

Breaking Changes / Porting Guide
--------------------------------

- Removed kind from to credentials role. This will be depreciated in a few months. Kind arguments are replaced by the credential_type and inputs fields.
- Updated to allow use of either awx.awx or ansible.tower

Bugfixes
--------

- Corrected README for tower_validate_certs variable defaults on all roles

v1.0.2
======

Minor Changes
-------------

- added alias option for survey to survey_spec in workflows.
- updated documentation on surveys for workflows and job templates

v1.0.0
======

Major Changes
-------------

- Updated Roles to use the tower_export model from the awx command line.
- credential_types Updated to use the tower_export model from the awx command line.
- credentials Updated to use the tower_export model from the awx command line.
- inventory Updated to use the tower_export model from the awx command line.
- inventory_sources Updated to use the tower_export model from the awx command line.
- job_templates Updated to use the tower_export model from the awx command line.
- projects Updated to use the tower_export model from the awx command line.
- teams Updated to use the tower_export model from the awx command line.
- users Updated to use the tower_export model from the awx command line.

Minor Changes
-------------

- updated to allow vars in messages for notifications.
- updated tower workflows related role `workflow_job_templates` to include `survey_enabled` defaulting to `false` which is a module default and `omit` the `survey_spec` if not passed.
- updated various roles to include oauth token and tower config file.

Breaking Changes / Porting Guide
--------------------------------

- Removed depreciated options in inventory sources role (source_regions, instance_filters, group_by)
- Renamed notifications role to notification_templates role as in awx.awx:15.0. The variable is not tower_notification_templates.

v0.2.1
======

Minor Changes
-------------

- Changelog release cycle

v0.2.0
======

Minor Changes
-------------

- Added pre-commit hook for local development and automated testing purposes
- Standardised and corrected all READMEs

Bugfixes
--------

- Removed defaulted objects for all roles so that they were not always run if using a conditional against the variable. (see https://github.com/redhat-cop/tower_configuration/issues/68)

v0.1.0
======

Major Changes
-------------

- Groups role - Added groups role to the collection
- Labels role - Added labels role to the collection
- Notifications role - Added many options to notifications role
- Workflow Job Templates role - Added many options to WJT role

Minor Changes
-------------

- GitHub Workflows - Added workflows to run automated linting and integration tests against the codebase
- Hosts role - Added new_name and enabled options to hosts role
- Housekeeping - Added CONTRIBUTING guide and pull request template
- Inventory Sources role - Added notification_templates_started, success, and error options. Also added verbosity and source_regions options.
- Teams role - Added new_name option to teams role
- Test Configs - Added full range of test objects for integration testing

Bugfixes
--------

- Fixed an issue where tower_validate_certs and validate_certs were both used as vars. Now changed to tower_validate_certs
