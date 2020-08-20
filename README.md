# Student-Registration-System

The project was done in Netbeans IDE using the drag and drop feature ,so the code contains a lot of complicated portion of the internal functioning.
To make it easier , the 5 important methods that were manually coded by me are pasted below.
............................................................................................. 
       
                                #1 WHEN ADD BUTTON IS PRESSED


  private void buttonaddActionPerformed(java.awt.event.ActionEvent evt) {                                          
        
        String name=txtname.getText();
        String mobile=txtmobile.getText();
        String course=txtcourse.getText();
        try {
            Class.forName("com.mysql.jdbc.Driver");
            
          con=DriverManager.getConnection("jdbc:mysql://localhost/dps","root","");  
            
          //String query=
          st=con.prepareStatement("INSERT INTO record (NAME,MOBILE,COURSE)VALUES(?,?,?)");
          st.setString(1, name);
          st.setString(2, mobile);
          st.setString(3, course);//1 ,2 and 3 represent the columns respectively
          st.executeUpdate();
          //"NOW WE NEED A MESSAGE BOX !!!"
          JOptionPane.showMessageDialog(this,"Record Added");
          
          //now we will call the method that loads the contents of the database into the JTable
          table_update();
          
          //now we just clear the text boxes
          txtname.setText("");
          txtmobile.setText("");
          txtcourse.setText("");
          
          txtname.requestFocus();
          
          
          
          
            
        } catch (ClassNotFoundException | SQLException ex) {
            Logger.getLogger(reg.class.getName()).log(Level.SEVERE, null, ex);
        }
        
    }       
     
.....................................................................

 
                       #2- WHEN AN ENTRY ON JTABLE IS CLICKED (IS USED FOR EDIT/DELETE OF THAT ENTRY)
      
       private void jTable1MouseClicked(java.awt.event.MouseEvent evt) {                                     
        
        DefaultTableModel Df=(DefaultTableModel)jTable1.getModel();
        int selectedIndex=jTable1.getSelectedRow();
        
        txtname.setText((String) Df.getValueAt(selectedIndex,1));
        txtmobile.setText((String) Df.getValueAt(selectedIndex,2));
        txtcourse.setText((String) Df.getValueAt(selectedIndex,3));
        
    }                           
  
                                  
..............................................................
                                   #3 WHEN DELETE BUTTON IS PRESSED       
                                                       
        Connection con;
    PreparedStatement st;
   private void buttondeleteActionPerformed(java.awt.event.ActionEvent evt) {                                             
      
        DefaultTableModel Df=(DefaultTableModel)jTable1.getModel();
        int selectedIndex=jTable1.getSelectedRow();
        
         try {
             int id=(int) Df.getValueAt(selectedIndex,0);//we use Df to refer to the jTable1,, column 0 holds the ID
             int dialogResult = JOptionPane.showConfirmDialog(null,"Are you sure you want to delete the record?","WARNING!",JOptionPane.YES_NO_OPTION);
             if(dialogResult== JOptionPane.YES_OPTION)
             {
                 Class.forName("com.mysql.jdbc.Driver");
            
          con=DriverManager.getConnection("jdbc:mysql://localhost/dps","root","");  
          
          st=con.prepareStatement("DELETE FROM record WHERE ID=?");
          st.setInt(1, id);
          st.executeUpdate();
          JOptionPane.showMessageDialog(this, "Record has been deleted");
          table_update();            
             }
             txtname.setText("");
          txtmobile.setText("")
          txtcourse.setText("");
             
         
              
           
        } catch (ClassNotFoundException | SQLException ex) {
            Logger.getLogger(reg.class.getName()).log(Level.SEVERE, null, ex);
        }
    }           
 ..........................................................
                             #4 WHEN EDIT BUTTON IS PRESSED
                                                                   
                                                                   
 private void buttoneditActionPerformed(java.awt.event.ActionEvent evt) {                                           
      
         DefaultTableModel Df=(DefaultTableModel)jTable1.getModel();//command to get the current model of jTable1
        int selectedIndex=jTable1.getSelectedRow();//to get the selected row
        
        try {
            //now to refer to it in the database,we will have to find the actual value of id so we can write it in the where clause
            int id=(int) Df.getValueAt(selectedIndex,0);//we use Df to refer to the jTable1,, column 0 holds the ID
            String name=txtname.getText();
            String mobile=txtmobile.getText();
            String course=txtcourse.getText();
            
            Class.forName("com.mysql.jdbc.Driver");
            
          con=DriverManager.getConnection("jdbc:mysql://localhost/dps","root","");  
            
          st=con.prepareStatement("UPDATE record SET NAME=?,MOBILE=?,COURSE=? WHERE ID=?");
          st.setString(1, name);
          st.setString(2, mobile);
          st.setString(3, course);//1 ,2 and 3 represent the columns respectively
          st.setInt(4, id);
          st.executeUpdate();
          //"NOW WE NEED A MESSAGE BOX !!!"
          JOptionPane.showMessageDialog(this,"Record Edited");
          
          //now we will call the method that loads the contents of the database into the JTable
          table_update();
          
          //now we just clear the text boxes
          txtname.setText("");
          txtmobile.setText("");
          txtcourse.setText("");
          
          txtname.requestFocus();
          
          
          
          
            
        } catch (ClassNotFoundException | SQLException ex) {
            Logger.getLogger(reg.class.getName()).log(Level.SEVERE, null, ex);
        }
        
    }                                          
.................................................
                    #5 METHOD TO UPDATE THE JTABLE
                                                                    
  //method to load values from the database into the JTable
    private void table_update()
    {
        int c; //this will store number of columns
         try {
            Class.forName("com.mysql.jdbc.Driver");
            
          con=DriverManager.getConnection("jdbc:mysql://localhost/dps","root","");  
            
          
          st=con.prepareStatement("SELECT * FROM record");
          ResultSet rs=st.executeQuery();
          ResultSetMetaData Rss=rs.getMetaData();
          c=Rss.getColumnCount();//now we know the number of columns
          
          DefaultTableModel Df=(DefaultTableModel)jTable1.getModel();
          Df.setRowCount(0);
           
          while(rs.next())
          {
              Vector v2=new Vector();
              for(int i=1;i<=c;i++)
              {
                  v2.add(rs.getString("ID")); 
                  v2.add(rs.getString("NAME"));
                  v2.add(rs.getString("MOBILE"));
                  v2.add(rs.getString("COURSE"));
                  
              }
              Df.addRow(v2);
              
          }
              
          
          
      
            
        } catch (ClassNotFoundException | SQLException ex) {
            Logger.getLogger(reg.class.getName()).log(Level.SEVERE, null, ex);
        }
        
    }
    
    ........................................                                                                
