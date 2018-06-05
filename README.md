# my-first-blog
import ldap


import moduleNIT as mod
def search(login):
  
    LDAP_SERVER = 'ldap://192.168.22.32:389'
    LDAP_USERNAME = 'hbelaid@csi.local'
    LDAP_PASSWORD = '50ruederosiereARGENTEUIL?'
    baseDN = "dc=csi,dc=local"

    try:
        ldap_client = ldap.initialize(LDAP_SERVER)
        ldap_client.set_option(ldap.OPT_REFERRALS,0)
        ldap_client.simple_bind_s(LDAP_USERNAME, LDAP_PASSWORD)
       # ldap_client.unbind()
        
        
        res = ldap_client.search_s(baseDN, ldap.SCOPE_SUBTREE,'sAMAccountName='+login,['lockoutTime','Samaccountname'])
        #print(res[0][1]["lockoutTime"][0])
# if "sAMAccountName" not in res[0][1]:
#         return("ERROR")
#     elif "lockoutTime" in res[0][1]:
#         if "lockoutTime" not in res[0][1]:
#             return ("unlocked")
#         elif (res[0][1]["lockoutTime"][0])==b'0':
#             return ("unlocked")
#     else:
#             return ("locked")
      
        if "sAMAccountName" not in res[0][1]:
            return "Error"
        elif "lockoutTime" in res[0][1]:
            if res[0][1]['lockoutTime'][0] == b'0':
                return "UnLocked"
            else :
                return "Locked"
        else:
            return "Unlocked"

    except ldap.LDAPError:
        return False
    except ldap.INVALID_CREDENTIALS:
        ldap_client.unbind()
        return False
    except ldap.SERVER_DOWN:
        logger.error("LDAP ("+LDAP_SERVER+") down")
        

def unlocking(login):

    LDAP_SERVER = 'ldap://192.168.22.32:389'
    LDAP_USERNAME = 's_script@csi.local'
    LDAP_PASSWORD = ';Xd:?o!/srfeNn4^,qlH'
    baseDN = "dc=csi,dc=local"
    
    ldap_client = ldap.initialize(LDAP_SERVER)
    ldap_client.set_option(ldap.OPT_REFERRALS,0)
    ldap_client.simple_bind_s(LDAP_USERNAME, LDAP_PASSWORD)
    
    res = ldap_client.search_s(baseDN, ldap.SCOPE_SUBTREE,'sAMAccountName='+login,['lockoutTime','distinguishedName','sAMAccountName'])
    dn= res[0][1]["distinguishedName"][0]
    dm= res[0][1]["lockoutTime"][0]
    dl= res[0][1]["sAMAccountName"][0]
    

    ldap_client.modify_s(str(bytes.decode(dn)), [(ldap.MOD_REPLACE, "lockoutTime", b"0")])
    print('{}{}'.format(dn,dm))
    
    var = search(str(bytes.decode(dn)))
    return var

search("msbihi")
   
