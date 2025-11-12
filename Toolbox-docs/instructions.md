#Step 1: Add to system/build.prop:
 + persist.sys.kaorios=kousei
------------------------
#Step 2: Import classes.dex to the last classes of framework.jar
------------------------
#Step 3: Modify classes of framework.jar
1.1 Class: ApplicationPackageManager
 + Method: hasSystemFeature(Ljava/lang/String;)Z

Above:
    return v0
.end method

Add:
    invoke-static {v0, p1}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosAttestationBL(ZLjava/lang/String;)Z

    move-result v0

 + Method hasSystemFeature(Ljava/lang/String;I)Z

Above:
    return v0
.end method

Add:
    invoke-static {p1, p2, v0}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosFeatures(Ljava/lang/String;IZ)Z

    move-result v0

1.2 Class: Instrumentation
 + Method: newApplication(Ljava/lang/Class;Landroid/content/Context;)Landroid/app/Application;

Above:
    return-object v0
.end method

Add:
    invoke-static {p1}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosProps(Landroid/content/Context;)V

 + Method: newApplication(Ljava/lang/ClassLoader;Ljava/lang/String;Landroid/content/Context;)Landroid/app/Application;

Above:
    return-object v0
.end method

Add:
    invoke-static {p3}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosProps(Landroid/content/Context;)V

1.2 Class: KeyStore2
 + Method: getKeyEntry(Landroid/system/keystore2/KeyDescriptor;)Landroid/system/keystore2/KeyEntryResponse;

Above:
    return-object v0
.end method

Add:
    invoke-static {v0}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosKeybox(Landroid/system/keystore2/KeyEntryResponse;)Landroid/system/keystore2/KeyEntryResponse;

    move-result-object v0

1.3 Class: AndroidKeyStoreSpi
 + Method: engineGetCertificateChain(Ljava/lang/String;)[Ljava/security/cert/Certificate;

Below:
    .registers XX

Add:
    invoke-static {}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosPropsEngineGetCertificateChain()V

