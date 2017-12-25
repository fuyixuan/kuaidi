USE [master]
GO
/****** Object:  Database [express]    Script Date: 12/11/2017 22:36:32 ******/
CREATE DATABASE [express] ON  PRIMARY 
( NAME = N'express', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\express.mdf' , SIZE = 3072KB , MAXSIZE = UNLIMITED, FILEGROWTH = 1024KB )
 LOG ON 
( NAME = N'express_log', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL10_50.MSSQLSERVER\MSSQL\DATA\express_log.ldf' , SIZE = 1024KB , MAXSIZE = 2048GB , FILEGROWTH = 10%)
GO
ALTER DATABASE [express] SET COMPATIBILITY_LEVEL = 100
GO
IF (1 = FULLTEXTSERVICEPROPERTY('IsFullTextInstalled'))
begin
EXEC [express].[dbo].[sp_fulltext_database] @action = 'enable'
end
GO
ALTER DATABASE [express] SET ANSI_NULL_DEFAULT OFF
GO
ALTER DATABASE [express] SET ANSI_NULLS OFF
GO
ALTER DATABASE [express] SET ANSI_PADDING OFF
GO
ALTER DATABASE [express] SET ANSI_WARNINGS OFF
GO
ALTER DATABASE [express] SET ARITHABORT OFF
GO
ALTER DATABASE [express] SET AUTO_CLOSE OFF
GO
ALTER DATABASE [express] SET AUTO_CREATE_STATISTICS ON
GO
ALTER DATABASE [express] SET AUTO_SHRINK OFF
GO
ALTER DATABASE [express] SET AUTO_UPDATE_STATISTICS ON
GO
ALTER DATABASE [express] SET CURSOR_CLOSE_ON_COMMIT OFF
GO
ALTER DATABASE [express] SET CURSOR_DEFAULT  GLOBAL
GO
ALTER DATABASE [express] SET CONCAT_NULL_YIELDS_NULL OFF
GO
ALTER DATABASE [express] SET NUMERIC_ROUNDABORT OFF
GO
ALTER DATABASE [express] SET QUOTED_IDENTIFIER OFF
GO
ALTER DATABASE [express] SET RECURSIVE_TRIGGERS OFF
GO
ALTER DATABASE [express] SET  DISABLE_BROKER
GO
ALTER DATABASE [express] SET AUTO_UPDATE_STATISTICS_ASYNC OFF
GO
ALTER DATABASE [express] SET DATE_CORRELATION_OPTIMIZATION OFF
GO
ALTER DATABASE [express] SET TRUSTWORTHY OFF
GO
ALTER DATABASE [express] SET ALLOW_SNAPSHOT_ISOLATION OFF
GO
ALTER DATABASE [express] SET PARAMETERIZATION SIMPLE
GO
ALTER DATABASE [express] SET READ_COMMITTED_SNAPSHOT OFF
GO
ALTER DATABASE [express] SET HONOR_BROKER_PRIORITY OFF
GO
ALTER DATABASE [express] SET  READ_WRITE
GO
ALTER DATABASE [express] SET RECOVERY FULL
GO
ALTER DATABASE [express] SET  MULTI_USER
GO
ALTER DATABASE [express] SET PAGE_VERIFY CHECKSUM
GO
ALTER DATABASE [express] SET DB_CHAINING OFF
GO
EXEC sys.sp_db_vardecimal_storage_format N'express', N'ON'
GO
USE [express]
GO
/****** Object:  Table [dbo].[kd]    Script Date: 12/11/2017 22:36:33 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[kd](
	[id] [int] IDENTITY(1,1) NOT NULL,
	[no] [varchar](500) NULL,
	[qssj] [varchar](500) NULL,
	[qsqk] [varchar](500) NULL,
PRIMARY KEY CLUSTERED 
(
	[id] ASC
)WITH (PAD_INDEX  = OFF, STATISTICS_NORECOMPUTE  = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS  = ON, ALLOW_PAGE_LOCKS  = ON) ON [PRIMARY]
) ON [PRIMARY]
GO
SET ANSI_PADDING OFF
GO
SET IDENTITY_INSERT [dbo].[kd] ON
INSERT [dbo].[kd] ([id], [no], [qssj], [qsqk]) VALUES (1, N'123456789', N'2017-12-12', N'已发货')
INSERT [dbo].[kd] ([id], [no], [qssj], [qsqk]) VALUES (2, N'123123123', N'2017-11-11', N'已拒收')
INSERT [dbo].[kd] ([id], [no], [qssj], [qsqk]) VALUES (3, N'111111111', N'2017-9-5', N'正在发往上海分拨中心')
INSERT [dbo].[kd] ([id], [no], [qssj], [qsqk]) VALUES (4, N'121212121', N'2017-11-11', N'包裹正在处理')


SET IDENTITY_INSERT [dbo].[kd] OFF

Eclipse代码：
package dao;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;
import java.util.Vector;

import pojo.Express;
import pojo.Student;
public class Dao
{
	private Connection dbc = null;
	private PreparedStatement ps = null;
	private ResultSet rs;
	
	public static Dao dao;
	
	private Dao(){
		try {
			Class.forName("net.sourceforge.jtds.jdbc.Driver");
			dbc = DriverManager.getConnection("jdbc:jtds:sqlserver://127.0.0.1:1433;DatabaseName=express", "sa", "1234");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
	
	public static Dao getDao()
	{
		if (dao == null)
		{
			dao = new Dao();
		}
		return dao;
	}

	
	public Express info(String no)
	{
		Express q = null;
		String sql = "select id,no,qssj,qsqk from kd where no ='"+no+"'";
		System.out.println(sql);
		try {
			ps = dbc.prepareStatement(sql);
			rs = ps.executeQuery();
			while (rs.next())
			{
				q = new Express();
				q.setId(rs.getInt(1));
				q.setNo(rs.getString(2));
				q.setQssj(rs.getString(3));
				q.setQsqk(rs.getString(4));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return q;
	}
	
	
}

/*
 * Main.java
 *
 * Created on __DATE__, __TIME__
 */

package main;

import javax.swing.JOptionPane;

import pojo.Express;

import dao.Dao;

/**
 *
 * @author  __USER__
 */
public class Main extends javax.swing.JFrame {

	/** Creates new form Main */
	public Main() {
		initComponents();
	}

	//GEN-BEGIN:initComponents
	// <editor-fold defaultstate="collapsed" desc="Generated Code">
	private void initComponents() {

		jLabel1 = new javax.swing.JLabel();
		jTextField1 = new javax.swing.JTextField();
		jLabel2 = new javax.swing.JLabel();
		jScrollPane1 = new javax.swing.JScrollPane();
		jTextPane1 = new javax.swing.JTextPane();
		jTextPane1.setEditable(false);
		jButton1 = new javax.swing.JButton();
		jButton2 = new javax.swing.JButton();

		setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);
		getContentPane().setLayout(
				new org.netbeans.lib.awtextra.AbsoluteLayout());

		jLabel1.setText("单号");
		getContentPane().add(
				jLabel1,
				new org.netbeans.lib.awtextra.AbsoluteConstraints(100, 50, -1,-1));
		getContentPane().add(
				jTextField1,
				new org.netbeans.lib.awtextra.AbsoluteConstraints(150, 50, 300,-1));

		jLabel2.setText("圆通的快递一般为10个数字，以1、2、6、8及w等开头");
		getContentPane().add(jLabel2,new org.netbeans.lib.awtextra.AbsoluteConstraints(100, 100,
						380, -1));

		jScrollPane1.setViewportView(jTextPane1);

		getContentPane().add(
				jScrollPane1,
				new org.netbeans.lib.awtextra.AbsoluteConstraints(100, 130,
						360, 230));

		jButton1.setText("确定");
		jButton1.addActionListener(new java.awt.event.ActionListener() {
			public void actionPerformed(java.awt.event.ActionEvent evt) {
				jButton1ActionPerformed(evt);
			}
		});
		getContentPane().add(
				jButton1,
				new org.netbeans.lib.awtextra.AbsoluteConstraints(160, 390, -1,-1));

		jButton2.setText("退出");
		jButton2.addActionListener(new java.awt.event.ActionListener() {
			public void actionPerformed(java.awt.event.ActionEvent evt) {
				jButton2ActionPerformed(evt);
			}
		});
		getContentPane().add(
				jButton2,
				new org.netbeans.lib.awtextra.AbsoluteConstraints(290, 390, -1,-1));

		this.setSize(550,480);
	}// </editor-fold>
	//GEN-END:initComponents

	private void jButton2ActionPerformed(java.awt.event.ActionEvent evt) {
		System.exit(-1);
	}

	private void jButton1ActionPerformed(java.awt.event.ActionEvent evt) {
		Dao dao = Dao.getDao();
		String no = jTextField1.getText();
		if (no != null && !no.equals(""))
		{
			Express exp = dao.info(no);
			if (exp != null)
			{
				jTextPane1.setText("快递存在是否：存在"+"\n"+"签收时间: "+exp.getQssj()+"\n"+"快递签收情况: "+exp.getQsqk());
			}else
			{
				jTextPane1.setText("快递存在是否：不存在");
			}
			
		}else
		{
			JOptionPane.showMessageDialog(null,"请输入单号");
		}
	}

	/**
	 * @param args the command line arguments
	 */
	public static void main(String args[]) {
		java.awt.EventQueue.invokeLater(new Runnable() {
			public void run() {
				new Main().setVisible(true);
			}
		});
	}

	//GEN-BEGIN:variables
	// Variables declaration - do not modify
	private javax.swing.JButton jButton1;
	private javax.swing.JButton jButton2;
	private javax.swing.JLabel jLabel1;
	private javax.swing.JLabel jLabel2;
	private javax.swing.JScrollPane jScrollPane1;
	private javax.swing.JTextField jTextField1;
	private javax.swing.JTextPane jTextPane1;
	// End of variables declaration//GEN-END:variables

}


package pojo;
public class Express {
	private int id;
	private String no;
	private String qssj;
	private String qsqk;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getNo() {
		return no;
	}
	public void setNo(String no) {
		this.no = no;
	}
	public String getQssj() {
		return qssj;
	}
	public void setQssj(String qssj) {
		this.qssj = qssj;
	}
	public String getQsqk() {
		return qsqk;
	}
	public void setQsqk(String qsqk) {
		this.qsqk = qsqk;
	}
}
