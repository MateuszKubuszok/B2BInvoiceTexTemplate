Polish B2B Invoice TEX template
===

Example
---

You can see example PDF [here](https://github.com/MateuszKubuszok/B2BInvoiceTexTemplate/releases/download/0.2/example.pdf). It uses example TeX below as input.

How to use
---

Create commands for your own company as `.sty` file (in example we named it `issuer.sty`):

    \ProvidesPackage{issuer}

    \newcommand{\Issuer}[0]{
    ExampleSoft Jan Kowalski \\
    ul. Papierowa 1 \\
    69-666 Warszawa \\
    NIP: 1234567890 \\
    REGON: 123456789
    }

    \newcommand{\IssuerBankAccount}{
    ExampleSoft Jan Kowalski \\
    ul. Papierowa 1 \\
    69-666 Warszawa \\
    Bank Bankowy \\
    99 1050 1000 1000 0000 1111 2222
    }

    \newcommand{\IssuerInvoiceSigner}{Jan Kowalski}

Similarly we create commands for generating client data as another `.sty` file (in this example `client.sty`):

    \ProvidesPackage{scalac}

    \newcommand{\Client}[0]{
    Klient Sp. z o.o. \\
    ul. Papierowa 2 \\
    69-666 Warszawa \\
    NIP: 0987654321 \\
    REGON: 987654321
    }

Now all we need to do is to create an invoce with them:

    \documentclass[12pt,a4paper]{article}
    \usepackage{invoice}
    \usepackage{issuer} % to invoice with another issuer you can simply use some different package
    \usepackage{client} % similarly here you can invoice another client simply by swapping packages

    \pagestyle{empty}

    \begin{document}
    \begin{invoice}[Number=2015/1, % invoice number - required to be unique, but there are not strict format guidelones
                    SellingDate=30.02.2015, 
                    InvoiceDate=30.02.2015,
                    PaymentDate=14.03.2015,
                    Seller=\Issuer, % here you can simply use command to fill invoice issuer data for you
                    Buyer=\Client, % here you can simply use command to fill client data for you
                    TotalNr=1770.00 zł, % unfortunatelly you have to fill total on your own
                    TotalWords=tysiąc siedemset siedemdziesiąt złotych, % same here
                    BankAccount=\IssuerBankAccount, % here you can use command to fill bank account for you
                    Signer=\IssuerInvoiceSigner] % here you can use command to fill invoice signer for you

      % Invoice has 6 columns to fill:
      %  1. item number,
      %  2. service description,
      %  3. price netto
      %  4. VAT tax
      %  5. VAT tax value
      %  6. price brutto

      1. &
      Usługi dla klienta &
      1000.00 zł &
      23\% &
      \tax{1000}{23} zł & % \tax{netto}{vat} and \brutto{netto}{vat} can make things easier to read
      \brutto{1000}{23} zł \\
      
      \hline
      
      2. &
      Delegacja (bilety lotnicze) &
      500.00 zł &
      8\% &
      \tax{500}{8} zł &
      \brutto{500}{8} zł \\
    \end{invoice}
    \end{document}

Then use something like TexStudio to generate your invoice, and make sure your accountant checks it before sending it out to client.
