
## Step 1: Add to `system/build.prop`

```properties
persist.sys.kaorios=kousei
```

---

## Step 2: Import `classes.dex`

Import **classes.dex** của Kaorios vào **cuối cùng** trong danh sách `classes` của `framework.jar`.

---

## Step 3: Modify `framework.jar` Classes

### 3.1 Class: `ApplicationPackageManager`

#### ➤ Method: `hasSystemFeature(Ljava/lang/String;)Z`

**Tìm dòng:**
```smali
return v0
.end method
```

**Chèn *ngay phía trên*:**
```smali
invoke-static {v0, p1}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosAttestationBL(ZLjava/lang/String;)Z
move-result v0
```

---

#### ➤ Method: `hasSystemFeature(Ljava/lang/String;I)Z`

**Tìm dòng:**
```smali
return v0
.end method
```

**Chèn *ngay phía trên*:**
```smali
invoke-static {p1, p2, v0}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosFeatures(Ljava/lang/String;IZ)Z
move-result v0
```

---

### 3.2 Class: `Instrumentation`

#### ➤ Method: `newApplication(Ljava/lang/Class;Landroid/content/Context;)Landroid/app/Application;`

**Tìm dòng:**
```smali
return-object v0
.end method
```

**Chèn *ngay phía trên*:**
```smali
invoke-static {p1}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosProps(Landroid/content/Context;)V
```

---

#### ➤ Method: `newApplication(Ljava/lang/ClassLoader;Ljava/lang/String;Landroid/content/Context;)Landroid/app/Application;`

**Tìm dòng:**
```smali
return-object v0
.end method
```

**Chèn *ngay phía trên*:**
```smali
invoke-static {p3}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosProps(Landroid/content/Context;)V
```

---

### 3.3 Class: `KeyStore2`

#### ➤ Method: `getKeyEntry(Landroid/system/keystore2/KeyDescriptor;)Landroid/system/keystore2/KeyEntryResponse;`

**Tìm dòng:**
```smali
return-object v0
.end method
```

**Chèn *ngay phía trên*:**
```smali
invoke-static {v0}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosKeybox(Landroid/system/keystore2/KeyEntryResponse;)Landroid/system/keystore2/KeyEntryResponse;
move-result-object v0
```

---

### 3.4 Class: `AndroidKeyStoreSpi`

#### ➤ Method: `engineGetCertificateChain(Ljava/lang/String;)[Ljava/security/cert/Certificate;`

**Ngay sau phần khai báo registers (`.registers XX`)**, thêm dòng:

```smali
invoke-static {}, Lcom/android/internal/util/kaorios/ToolboxUtils;->KaoriosPropsEngineGetCertificateChain()V
```

---
