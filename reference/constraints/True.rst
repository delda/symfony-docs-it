True
====

Valida che un valore is ``true``. Specifically, this checks to see if
the value is exactly ``true``, exactly the integer ``1``, or exactly the
string "``1``".

Also see :doc:`False <False>`.

+----------------+---------------------------------------------------------------------+
| Si applica a   | :ref:`property or method<validation-property-target>`               |
+----------------+---------------------------------------------------------------------+
| Opzioni        | - `message`_                                                        |
+----------------+---------------------------------------------------------------------+
| Classe         | :class:`Symfony\\Component\\Validator\\Constraints\\True`           |
+----------------+---------------------------------------------------------------------+
| Validatore     | :class:`Symfony\\Component\\Validator\\Constraints\\TrueValidator`  |
+----------------+---------------------------------------------------------------------+

Uso di base
-----------

This constraint can be applied to properties (p.e. a ``termsAccepted`` property
on a registration model) or to a "getter" method. It's most powerful in the
latter case, where you can assert that a method returns a true value. For
example, suppose you have the following method:

.. code-block:: php

    // src/Acme/BlogBundle/Entity/Author.php
    namespace Acme\BlogBundle\Entity;

    class Author
    {
        protected $token;

        public function isTokenValid()
        {
            return $this->token == $this->generateToken();
        }
    }

Then you can constrain this method with ``True``.

.. configuration-block::

    .. code-block:: yaml

        # src/Acme/BlogBundle/Resources/config/validation.yml
        Acme\BlogBundle\Entity\Author:
            getters:
                tokenValid:
                    - "True": { message: "The token is invalid" }

    .. code-block:: php-annotations

        // src/Acme/BlogBundle/Entity/Author.php
        use Symfony\Component\Validator\Constraints as Assert;

        class Author
        {
            protected $token;

            /**
             * @Assert\True(message = "The token is invalid")
             */
            public function isTokenValid()
            {
                return $this->token == $this->generateToken();
            }
        }

    .. code-block:: xml

        <?xml version="1.0" encoding="UTF-8" ?>
        <!-- src/Acme/Blogbundle/Resources/config/validation.xml -->

        <constraint-mapping xmlns="http://symfony.com/schema/dic/constraint-mapping"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://symfony.com/schema/dic/constraint-mapping http://symfony.com/schema/dic/constraint-mapping/constraint-mapping-1.0.xsd">

            <class name="Acme\BlogBundle\Entity\Author">
                <getter property="tokenValid">
                    <constraint name="True">
                        <option name="message">The token is invalid...</option>
                    </constraint>
                </getter>
            </class>
        </constraint-mapping>

    .. code-block:: php

        // src/Acme/BlogBundle/Entity/Author.php
        use Symfony\Component\Validator\Mapping\ClassMetadata;
        use Symfony\Component\Validator\Constraints\True;
        
        class Author
        {
            protected $token;
            
            public static function loadValidatorMetadata(ClassMetadata $metadata)
            {
                $metadata->addGetterConstraint('tokenValid', new True(array(
                    'message' => 'The token is invalid',
                )));
            }

            public function isTokenValid()
            {
                return $this->token == $this->generateToken();
            }
        }

If the ``isTokenValid()`` returns false, the validation will fail.

Opzioni
-------

message
~~~~~~~

**tipo**: ``stringa`` **predefinito**: ``This value should be true``

Messaggio mostrato se the underlying data is not true.