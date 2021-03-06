.. index::
    single: Options Resolver
    single: Componenti; OptionsResolver

Il componente OptionsResolver
=============================

    Il Componente OptionsResolver aiuta a configurare gli oggetti con un array
    di opzioni. Esso supporta valori predefiniti, opzioni con vincoli e opzioni pigre.

Installazione
-------------

È possibile installare i componente in due modi differenti:

* utilizzare il repository ufficiale Git (https://github.com/symfony/OptionsResolver
* :doc:`installare il componente via Composer </components/using_components>` (``symfony/options-resolver`` su `Packagist`_)

Utilizzo
--------

Si immagini di avere una classe ``Mailer`` che ha 2 opzioni: ``host`` e
``password``. Queste opzioni stanno per essere gestite dal Componente 
OptionsResolver.

Innanzitutto, creare la classe ``Mailer``::

    class Mailer
    {
        protected $options;

        public function __construct(array $options = array())
        {
        }
    }

Si potrebbe settare il valore di ``$options`` direttamente nella proprietà. Invece,
utilizzare la classe :class:`Symfony\\Component\\OptionsResolver\\OptionsResolver`
e lasciare che essa risolva le opzioni tramite la chiamata al metodo
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::resolve`.
I vantaggi di operare in questo modo saranno più ovvi andando avanti::

    use Symfony\Component\OptionsResolver\OptionsResolver;

    // ...
    public function __construct(array $options = array())
    {
        $resolver = new OptionsResolver();

        $this->options = $resolver->resolve($options);
    }

La proprietà ``$options`` è un'istanza di
:class:`Symfony\\Component\\OptionsResolver\\Options`, la quale implementa
:phpclass:`ArrayAccess`, :phpclass:`Iterator` e :phpclass:`Countable`. Questo
significa che è possibile gestirla come un normale array::

    // ...
    public function getHost()
    {
        return $this->options['host'];
    }

    public function getPassword()
    {
        return $this->options['password'];
    }

Configurare OptionsResolver
---------------------------

Adesso, prova ad utilizzare effettivamente la classe::

    $mailer = new Mailer(array(
        'host'     => 'smtp.example.org',
        'password' => 'pa$$word',
    ));

    echo $mailer->getPassword();

In questo momento, avrai ricevuto una 
:class:`Symfony\\Component\\OptionsResolver\\Exception\\InvalidOptionsException`,
la quale ci informa che le opzioni ``host`` e ``password`` non esistono.
Questo perché è necessario configurare prima l'``OptionsResolver``, in modo che
sappia quali opzioni devono essere risolte.

.. tip::

    Per controllare se un'opzione esiste, si può utilizzare la
    funzione :method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::isKnown`.


Una buona pratica è porre la configurazione in un metodo (per esempio
``setDefaultOptions``). Il metodo viene invocato nel costruttore per configurare
la classe ``OptionsResolver``::

    use Symfony\Component\OptionsResolver\OptionsResolver;
    use Symfony\Component\OptionsResolver\OptionsResolverInterface;

    class Mailer
    {
        protected $options;

        public function __construct(array $options = array())
        {
            $resolver = new OptionsResolver();
            $this->setDefaultOptions($resolver);

            $this->options = $resolver->resolve($options);
        }

        protected function setDefaultOptions(OptionsResolverInterface $resolver)
        {
            // ... configura il resolver, come si apprendererà nelle sezioni successive
        }
    }

Opzioni Obbligatorie
--------------------

Supponiamo che l'opzione ``firstName`` sia obbligatoria: la classe non può funzionare senza
di essa. Si possono settare le opzioni obbligatorie invocando
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::setRequired`::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        $resolver->setRequired(array('host'));
    }

A questo punto è possible usare la classe senza errori::

    $mailer = new Mailer(array(
        'host' => 'smtp.example.org',
    ));

    echo $mailer->getHost(); // 'smtp.example.org'

Se un'opzione obbligatoria non viene passata, una
:class:`Symfony\\Component\\OptionsResolver\\Exception\\MissingOptionsException`
sarà lanciata.

Per determinare se un'opzione è obbligatoria, si può usare il
metodo
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::isRequired`.

Opzioni Facoltative
-------------------

Qualche volta, un'opzione può essere facoltativa (per esempio l'opzione ``lastName`` nella classe
``Person``). È possibile configurare queste opzioni invocando
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::setOptional`::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setOptional(array('password'));
    }

Settare Valori Predefiniti
--------------------------

La maggior parte delle opzioni facoltative hanno un valore predefinito. È possibile configurare queste
opzioni invocando
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::setDefaults`::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setDefaults(array(
            'username' => 'root',
        ));
    }

Questo aggiunge una terza opzione, ``username``, con valore predefinito
``root``. Quando l'utente specifica uno ``username``, il valore predefinito
viene sovrascritto. Non è necessario configurare ``username`` come una opzione facoltativa.
``OptionsResolver`` sa già che le opzioni con un valore predefinito sono
facoltative.

Il componente ``OptionsResolver`` ha anche un
metodo :method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::replaceDefaults`. 
Questo può essere usato per sovrascrivere il valore precedente. La closure
che è passata ha due parametri:

* ``$options`` (un'istanza di :class:`Symfony\\Component\\OptionsResolver\\Options`), 
  con tutti i valori predefiniti
* ``$value``, il set precedente di valori predefiniti

Valori Predefiniti che dipendono da altre Opzioni
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Supponiamo di aggiungere un'opzione ``gender`` alla classe ``Person``, il cui valore predefinito
è indovinato sulla base del nome. È possibile fare questo facilmente usando una
Closure come valore predefinito::

    use Symfony\Component\OptionsResolver\Options;

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setDefaults(array(
            'port' => function (Options $options) {
                if (in_array($options['host'], array('127.0.0.1', 'localhost')) {
                    return 80;
                }

                return 25;
            },
        ));
    }

.. caution::

    Il primo argomento della Closure deve essere di tipo ``Options``,
    altrimenti sarà considerata come il valore.

Configurare i Valori consentiti
-------------------------------

Non tutti i valori sono validi per le opzioni. Per esempio, l'opzione ``gender``
può essere solo ``female`` o ``male``. È possibile configurare questi valori consentiti
invocando
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::setAllowedValues`::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setAllowedValues(array(
            'transport' => array('sendmail', 'mail', 'smtp'),
        ));
    }

Esiste anche un metodo
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::addAllowedValues`, 
che è possibile utilizzare se si vuole aggiungere un valore consentito al precedente
set di valori consentiti.

Configurare i Tipi consentiti
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

È possibile anche specificare i valori consentiti. Per esempio, l'opzione ``firstName`` può
essere qualsiasi cosa, ma deve essere una stringa. È possibile configurare questi tipi invocando
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::setAllowedTypes`::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setAllowedTypes(array(
            'port' => 'integer',
        ));
    }

I possibili tipi sono quelli associati alle funzioni php ``is_*`` o al nome
della classe. È possibile passare anche un array di tipi come valore. Per esempio,
``array('null', 'string')`` consente a ``port`` di essere ``null`` o una stringa.

Esiste anche un metodo
:method:`Symfony\\Component\\OptionsResolver\\OptionsResolver::addAllowedTypes`, 
che può essere utilizzato per aggiungere un tipo consentito a quelli precedentemente indicati.

Normalizzare le Opzioni
-----------------------

Alcuni valori devono essere normalizzati prima che possano essere usati. Per esempio,
``firstName`` dovrebbe sempre iniziare con una lettera maiuscola. Per fare ciò, si posso 
scrivere dei normalizzatori. Queste Closure saranno eseguite dopo che tutte le opzioni sono state
passate e ritornano il valore normalizzato. I normalizzatori possono essere configurati
invocando
:method:`Symfony\\Components\\OptionsResolver\\OptionsResolver::setNormalizers`::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setNormalizers(array(
            'host' => function (Options $options, $value) {
                if ('http://' !== substr($value, 0, 7)) {
                    $value = 'http://'.$value;
                }

                return $value;
            },
        ));
    }

È possibile notare che la closure riceve un parametetro ``$options``. Qualche volta, è
necessario utilizzare altre opzioni per normalizzare::

    // ...
    protected function setDefaultOptions(OptionsResolverInterface $resolver)
    {
        // ...

        $resolver->setNormalizers(array(
            'host' => function (Options $options, $value) {
                if (!in_array(substr($value, 0, 7), array('http://', 'https://')) {
                    if ($options['ssl']) {
                        $value = 'https://'.$value;
                    } else {
                        $value = 'http://'.$value;
                    }
                }

                return $value;
            },
        ));
    }

.. _Packagist: https://packagist.org/packages/symfony/options-resolver
