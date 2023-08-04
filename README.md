                      Used to set the connections of each page.
<?xml version="1.0"?>
<!-- 
    Note: As an alternative to hand editing this file you can use the 
    web admin tool to configure settings for your application. Use
    the Website->Asp.Net Configuration option in Visual Studio.
    A full list of settings and comments can be found in 
    machine.config.comments usually located in 
    \Windows\Microsoft.Net\Framework\v2.x\Config 
-->
<configuration>
	<appSettings>
		<add key="ConnStr" value="data source=RAMYA-2DCA5B123;database=BloodBequeathFederalAgent;integrated security=sspi"/>
	</appSettings>
	<connectionStrings>
  <add name="BloodDonationAgentConnectionString" connectionString="Data Source=RAMYA-2DCA5B123;Initial Catalog=BloodDonationAgent;integrated security=sspi"
   providerName="System.Data.SqlClient" />
 </connectionStrings>
	<system.web>
		<!-- 
            Set compilation debug="true" to insert debugging 
            symbols into the compiled page. Because this 
            affects performance, set this value to true only 
            during development.
        -->
		<compilation debug="true">
			<assemblies>
				<add assembly="System.Design, Version=2.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/>
				<add assembly="System.Web.Extensions, Version=1.0.61025.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/>
				<add assembly="System.Web.Extensions.Design, Version=1.0.61025.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/>
				<add assembly="System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/></assemblies></compilation>
		<!--
            The <authentication> section enables configuration 
            of the security authentication mode used by 
            ASP.NET to identify an incoming user. 
        -->
		<authentication mode="Windows"/>
		<!--
            The <customErrors> section enables configuration 
            of what to do if/when an unhandled error occurs 
            during the execution of a request. Specifically, 
            it enables developers to configure html error pages 
            to be displayed in place of a error stack trace.

        <customErrors mode="RemoteOnly" defaultRedirect="GenericErrorPage.htm">
            <error statusCode="403" redirect="NoAccess.htm" />
            <error statusCode="404" redirect="FileNotFound.htm" />
        </customErrors>
        -->
	</system.web>
</configuration>

                                        User Login Form
using System;
using System.Data;
using System.Configuration;
using System.Collections;
using System.Web;
using System.Web.Security;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Web.UI.WebControls.WebParts;
using System.Web.UI.HtmlControls;
public partial class Login : System.Web.UI.Page
{
    CheckUser user = new CheckUser();
    UserAccountBusinessLayer account = new UserAccountBusinessLayer();
    OrganizationAccountBusinessLayer org = new OrganizationAccountBusinessLayer();
    protected void Page_Load(object sender, EventArgs e)
    {
        txtUsername.Focus();
    }

    protected void btnLogin_Click(object sender, EventArgs e)
    {
        try
        {
            user.Username = txtUsername.Text;
            user.Password = txtPassword.Text;
            //Check User
            
            if (user.GetUser() == true)
            {
                account.Username = txtUsername.Text;
                DataSet ds = new DataSet();
                ds = account.GetAccountId();
                string AcId = ds.Tables[0].Rows[0][0].ToString();
                Session["username"] = txtUsername.Text;
                Session["Acid"] = AcId;
                DataSet ds1 = new DataSet();
                account.Accountid =int.Parse(AcId);
                ds1 = account.GetAddressId();
                Session["addid"] = ds1.Tables[0].Rows[0][0].ToString();
                Response.Redirect("~/Donor/DonorHome.aspx");
            }
            else
                Image2.Visible = true;
            lblMsg.Text = "Your Login Attempt Is Failed Plz try Again....!";
            txtPassword.Text = "";
            txtUsername.Focus();
            //Checking Organization
            if (user.GetOrganization() == true)
            {
                account.Username = txtUsername.Text;
                DataSet ds = new DataSet();
                ds = account.GetAccountId();
                string AcId = ds.Tables[0].Rows[0][0].ToString();
                Session["username"] = txtUsername.Text;
                Session["Acid"] = AcId;
                DataSet ds1 = new DataSet();
                org.Orgid =int.Parse(AcId);
                ds1 = org.GetOrgAddressId();
                Session["addid"]=ds1.Tables[0].Rows[0][0].ToString();
                Response.Redirect("~/Organization/OrganizationHome.aspx");
            }
            else
                Image2.Visible = true;
            lblMsg.Text = "Your Login Atempt Is Failed Plz try Again....!";
            txtPassword.Text = "";
            txtUsername.Focus();
            //Employee Checking
            if (user.CheckEmployee() == true)
            {
                account.Username = txtUsername.Text;
                DataSet ds = new DataSet();
                ds = account.GetAccountId();
                string AcId = ds.Tables[0].Rows[0][0].ToString();
                Session["username"] = txtUsername.Text;
                Session["Acid"] = AcId;
                Response.Redirect("~/CallCenter/CallCenterHome.aspx");
            }
       
    }
}
# blood-bank-management-system
