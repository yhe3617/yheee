package com.sun.content.server.operator.security.adaptor; 
 
import java.util.*; 
import com.sun.content.server.service.security.*; 
import com.sun.content.server.service.security.util.*; 
 
// import external required packages to connect to the directory service 
 
public class SampleExternalProxy 
{  
    // Create a Client facace for serach 
    private ExternalClientObject externalUserMgr; 
 
    // Create an instance of External Directorory server Client Proxy 
    public SampleExternalProxy() throws Exception { 
 
                // User the external package to instanciate the Proxu 
                // You need to use the client/server setting in the 
    // operatorproxy.properties 
                // The following assume you implemented a class OperatorProxyProperties 
    // to read the configuration values. 
 
		String hostname = OperatorProxyProperties.EXTERNAL_SERVER_ADDRESS;  
		String port = OperatorProxyProperties.EXTERNAL_SERVER_PORT;  
		String clientLoginId = OperatorProxyProperties.CLIENT_LOGIN;  
    	    String clientPassword = OperatorProxyProperties.CLIENT_PASSWRD; 
     
            // instan 
            try { 
			externalUserMgr = new ExternalClientObject(hostname, port, 
             clientLoginId, clientPassword); 
            } catch (Exception expt) { 
                // process the exception 
         throw expt; 
            } 
        } 
 
    public boolean searchUser(String userName) { 
 
        boolean found = false; 
		// Use the external User manager and search function for the given string 
    // name 
		// Typically the call will look like 
 		// found = externalUserMgr.searchUser(userName); 
 		// You may need to catch potential exception and display the appropriate 
    // message 
        return found; 
    } 
 
    public SampleUserImpl createUserFromExternal(String loginId)  
            throw Exception { 
 
		System.out.println("DEBUG:SampleExternal Proxy --- createUserFromExternal "); 
		System.out.println(" Creating a SampleUserImpl from External Directory ...."); 
		System.out.println(" Reference Login Id = "+loginId); 
 
        try { 
             
			// Assuming the external client has been created and connection is 
      // established 
			// The following call typically search for the UserName and return an 
      // External UserProfile 
			aUserProfileData aUPD = externalUserMgr.getUserProfileData(loginId); 
			String password = aUPD.getCredential(); 
			 
            // Create basic User information (firstname, 
            // middlename, lastname, 
            // address, etc.) 
            String firstName = aUPD.getFirstName(); 
            String lastName = aUPD.getLastName(); 
            String middleName = aUPD.getMiddleInitial(); 
            String gender = aUPD.getGender(); 
            String salutation = aUPD.getOccupation(); 
            String street1 = aUPD.getStreet(); 
            String street2 = aUPD.getStreetNumber(); 
            String city = aUPD.getCity(); 
            String state = aUPD.getState(); 
            String postalcode = aUPD.getZipCode(); 
            String country = aUPD.getCountry(); 
            String phone = aUPD.getFixedPhone(); 
 
            // Creating email information  
            String email = aUPD.getMailAddress(); 
 
            // Creating msisdn in this case Unique Device Id () 
            String uniqueDeviceId = aUPD.getMsisdn(); 
 
            // Creating Status data:User Enabled/Desabled and User is 
            // Prepay/Non-Prepay are valued 1/0 
 
            boolean enabled = false; 
            if (aUPD.getStatus().equals("1")) 
                enabled = true; 
  
            boolean prepay = false; 
            if (aUPD.getPrepayType().equals("1")) 
                prepay = true; 
 
            // Create activation and deactivation dates if provided 
            Date activatedate = new Date(); 
            Date deactivatedate = null; 
 
			// Create and return a Sample User Implementation 
            return new SampleUserImpl( 
                loginId, 
                password, 
                firstName, 
                lastName, 
                middleName, 
                gender, 
                street1, 
                street2, 
                city, 
                state, 
                postalcode, 
                country, 
                email, 
                phone, 
                activatedate, 
                deactivatedate, 
                salutation, 
                enabled, 
			    uniqueDeviceId, 
                prepay); 
 
        } catch (Exception ex) { 
            // procecss exception 
            throw ex; 
        } 
 
        return null; 
    } 
} 
