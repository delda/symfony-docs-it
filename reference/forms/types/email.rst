.. index::
   single: Form; Campi; email

Tipo di campo email
===================

Il campo ``email`` è un campo di testo reso usando il tag
``<input type="email" />`` di HTML5.

+---------------+---------------------------------------------------------------------+
| Reso come     | campo ``input`` ``email`` (casella di testo)                        |
+---------------+---------------------------------------------------------------------+
| Opzioni       | - `max_length`_                                                     |
| ereditate     | - `required`_                                                       |
|               | - `label`_                                                          |
|               | - `trim`_                                                           |
|               | - `read_only`_                                                      |
|               | - `disabled`_                                                       |
|               | - `error_bubbling`_                                                 |
|               | - `error_mapping`_                                                  |
|               | - `mapped`_                                                         |
+---------------+---------------------------------------------------------------------+
| Tipo genitore | :doc:`form</reference/forms/types/form>`                            |
+---------------+---------------------------------------------------------------------+
| Classe        | :class:`Symfony\\Component\\Form\\Extension\\Core\\Type\\EmailType` |
+---------------+---------------------------------------------------------------------+

Opzioni ereditate
-----------------

Queste opzioni sono ereditate dal tipo :doc:`form</reference/forms/types/form>`:

.. include:: /reference/forms/types/options/max_length.rst.inc

.. include:: /reference/forms/types/options/required.rst.inc

.. include:: /reference/forms/types/options/label.rst.inc

.. include:: /reference/forms/types/options/trim.rst.inc

.. include:: /reference/forms/types/options/read_only.rst.inc

.. include:: /reference/forms/types/options/disabled.rst.inc

.. include:: /reference/forms/types/options/error_bubbling.rst.inc

.. include:: /reference/forms/types/options/error_mapping.rst.inc

.. include:: /reference/forms/types/options/mapped.rst.inc
