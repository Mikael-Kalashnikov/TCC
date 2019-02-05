# TCC
Adicionar dados do banco ao pdf

 int cont = 0;
        OutputStream outPutStream = null;
        Random gerador = new Random();
        cont = gerador.nextInt();  
        com.itextpdf.text.Document document = new com.itextpdf.text.Document(PageSize.A4, 30, 20, 20, 30);
        try {
                    outPutStream = new FileOutputStream("instrumentos_utilizados.pdf");
                } catch (FileNotFoundException ex) {
                    Logger.getLogger(Instrumentos_utilizados.class.getName()).log(Level.SEVERE, null, ex);
                }
        try {
            PdfWriter.getInstance(document, new FileOutputStream(""+ cont +".pdf"));   
            document.open();
            PdfPTable tabela = new PdfPTable(8);
            PdfPCell cabecalho = new PdfPCell(new Paragraph("Instrumentos utilizados"));
            cabecalho.setHorizontalAlignment(com.itextpdf.text.Element.ALIGN_CENTER);
            cabecalho.setColspan(8);
            tabela.addCell(cabecalho);
            tabela.addCell("Instrumento");
            tabela.addCell("Cód.");
            tabela.addCell("Tipo");
            tabela.addCell("Cód. da reserva");
            tabela.addCell("Responsável");
            tabela.addCell("Data");
            tabela.addCell("Inicio");
            tabela.addCell("Termino");
            
            document.add(tabela);
                    
        } catch (FileNotFoundException | DocumentException ex) {
            System.out.println("Error:"+ex);
        }finally{
            document.close();
        }
        try {
            Desktop.getDesktop().open(new File(""+ cont +".pdf"));
        } catch (IOException ex) {
           System.out.println("Error:"+ex);
        }       
