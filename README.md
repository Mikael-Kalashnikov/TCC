# TCC
Adicionar dados do banco ao pdf

 try {
            // TODO add your handling code here:
            //procurar classe no projeto
            Class.forName("com.mysql.jdbc.Driver");

            //criar variável de conexão
            Connection conn;

            //cria uma conexão com o banco            
            conn = DriverManager.getConnection("jdbc:mysql://127.0.0.1/sgsm", "root", "");

            //cria uma string para consultar           
            String query = "Select\n" +
"IR.Codigo_Instrumento ,\n" +
"I.Tipo,\n" +
"i.Descricao ,\n" +
"IR.Codigo_Reserva ,\n" +
"u.nome ,\n" +
"R.Horario_Inicio ,\n" +
"R.Horario_Final ,\n" +
"R.Data_Para_Reserva \n" +
"from Reserva R, Instrumentos_Da_Reserva IR, Usuario U, Instrumento I\n" +
"where R.Codigo = IR.Codigo_Reserva and U.Matricula = R.Matricula_Usuario and IR.Codigo_Instrumento = I.Codigo;";

            //cria um comando 
            PreparedStatement cmd;
            cmd = conn.prepareStatement(query);

//            seta os valores na string "query"
            

            ResultSet rs;
            rs = cmd.executeQuery();

            DefaultTableModel model = (DefaultTableModel) jTable1.getModel();

            model.setRowCount(0);

            while (rs.next()) {

                model.addRow(
                        new Object[]{
                          
                            rs.getString("I.Tipo"),
                            rs.getInt("IR.Codigo_Instrumento"),
                            rs.getString("i.Descricao"),
                            rs.getString("IR.Codigo_Reserva"),
                            rs.getString("u.nome"),
                            rs.getString("R.Data_Para_Reserva"),
                            rs.getString("R.Horario_Inicio"),
                            rs.getString("R.Horario_Final"),

                           
                   
                        }
                );

            }

            System.out.println(cmd.toString());

        } catch (ClassNotFoundException ex) {
            System.out.println("Não foi possivel encontrar a classe!   " + ex);
        } catch (SQLException ex) {
            System.out.println("Ocorreu um erro SQL!" + ex.getMessage());
        }   
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
