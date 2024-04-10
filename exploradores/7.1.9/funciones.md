


### local-var05
Cantidad ordenada - Cantidad embarcada
~~~
 (sod_qty_ord - sod_qty_ship)  
~~~

### local-var04
~~~
    get_flete(so_cust,sod_part,sod_um,sod_qty_ord,sod_site,so_ord_date,1)
~~~

Funcion get_flete
~~~
function get_flete returns decimal(icust as char,ipart as char,ium as char,iqty as dec, isite as char,idate as date,iweight as dec):

def var detwt as dec.
def var freight_ok as log.
def var charge as dec.
def var curr as char init "CLP".
def var peso as dec.
def var frc_returns as char.

if iqty < 0 then frc_returns = "N".

find first ad_mstr no-lock where ad_domain = global_domain and ad_addr = icust no-error.
find first cm_mstr no-lock where cm_domain = global_domain and cm_addr = icust no-error.

 charge = 0.

find first pt_mstr no-lock where pt_domain = global_domain and pt_part = ipart no-error.
if avail pt_mstr then
do:
find first um_mstr no-lock where um_domain = global_domain and um_um = ium and um_alt_um = "KG" AND um_part = ipart no-error.
if avail um_mstr then assign peso = iqty * pt_ship_wt * um_conv.
end.

if cm_fr_terms <> "" and cm_fr_list <> "" then do:

  /* FREIGHT ZONE DETAIL */
        FIND FIRST frzd_det WHERE frzd_domain = global_domain 
                              AND frzd_fr_list  = cm_fr_list   
                              AND frzd_site     = isite   
                              AND (frzd_start  <= TODAY or frzd_start = ?) 
                              AND (frzd_end    >= TODAY or frzd_end   = ?) 
                              AND frzd_post_st <= ad_zip   
                              AND frzd_post_en >= ad_zip NO-LOCK NO-ERROR.
        
        IF NOT AVAILABLE frzd_det 
        THEN DO: 
            ASSIGN charge = 0.
            RETURN charge.
        END.
        
        /* FREIGHT CHARGE RECORD FOR WT BRACKET */
        FIND FIRST frcd_det WHERE
             frcd_domain  = global_domain   AND
             frcd_fr_list = cm_fr_List     and
             frcd_site    = isite     and
             frcd_curr    = curr      and
             frcd_zone    = frzd_zone and
             frcd_class   = ipart and 
             frcd_max_wt >= peso    and
             frcd_min_wt  < peso    and
             (frcd_start <= TODAY OR frcd_start = ?) and
             (frcd_end   >= TODAY OR frcd_end = ?) NO-LOCK NO-ERROR.
        
        /* FIND FREIGHT CHARGE RECORD REGARDLESS OF MIN WT BRACKET */
        IF NOT AVAILABLE frcd_det THEN
                find first frcd_det where
                frcd_domain  = global_domain   AND
                frcd_fr_list = cm_fr_list     and
                frcd_site    = isite     and
                frcd_curr    = curr      and
                frcd_zone    = frzd_zone and
                frcd_max_wt >= peso    and
                (frcd_start <= TODAY OR frcd_start = ?)  and
                (frcd_end   >= TODAY OR frcd_end = ?) no-lock no-error.
                if available frcd_det then do:
        
        IF (peso > 0 and peso < frcd_min_wt) or
           (peso < 0 and frc_returns = "N" and
           ((frcd_min_wt < 0 and peso < frcd_min_wt) or
            (frcd_min_wt > 0 and (peso * -1) < frcd_min_wt)))
        THEN DO:
                IF peso < 0 and frc_returns = "N" and frcd_min_wt > 0 then
                    peso = frcd_min_wt * -1.
                ELSE peso = frcd_min_wt.
        END.
        

        IF peso <> 0 THEN 
        ASSIGN charge = frcd_min_wtc + 
                        frcd_xtr_wtc * (peso - frcd_min_wt) + 
                        (frcd_tot_wtc * peso).
        END.

        return charge.

end.
~~~

### local-var01
(cantidad ordenada * precio)
~~~
    sod_qty_ord * sod_price
~~~

### local-var10
    get_data_admstr(so_slspsn[4],"NOMBRE")

### local-var12
    get_data_admstr(cm_slspsn[2],"NOMBRE")

### local-var13
    get_data_admstr(so_slspsn[1],"NOMBRE")

### local-var14
    get_data_admstr(cm_slspsn[3],"NOMBRE")


Funcion get_data_admstr
~~~   
    FUNCTION get_data_admstr RETURNS CHAR (iaddr AS CHAR,itipo AS CHAR):
        FIND FIRST ad_mstr NO-LOCK WHERE ad_domain = global_domain
                                    AND ad_addr   = iaddr NO-ERROR.
        IF AVAIL ad_mstr 
        THEN DO:
        IF itipo = "NOMBRE"     THEN RETURN ad_name  + ad_line1.
        IF itipo = "DIRECCION"  THEN RETURN ad_line2 + ad_line3.
        IF itipo = "RUT"        THEN RETURN substring(ad_pst_id,3).
        END.
        ELSE RETURN "".
    END.
~~~

### local-var03
    get_clase(cm_class)

Funcion get_clase
~~~
function get_clase returns character(iclase as char):

def var d_clase as char.

d_clase = "".
find first code_mstr no-lock where code_domain  = global_domain
                               and code_fldname = "cm_class"
                               and code_value   = iclase no-error.
if avail code_mstr then d_clase = trim(code_cmmt).

return d_clase.

end.
~~~

### local-var00
    get_tipo(cm_type)

funcion get_tipo
~~~
function get_tipo returns character(itype as char):

def var d_type as char.

d_type = "".
find first code_mstr no-lock where code_domain  = global_domain
                               and code_fldname = "cm_type"
                               and code_value   = itype no-error.
if avail code_mstr then d_type = trim(code_cmmt).

return d_type.

end.
~~~

### local-var02
    get_stat_desc(so_stat)

Funcion get_stat_desc
~~~
function get_stat_desc returns character(istat as char):

def var d_stat as char.

d_stat = "".
find first code_mstr no-lock where code_domain  = global_domain
                               and code_fldname = "so_stat"
                               and code_value   = istat no-error.
if avail code_mstr then d_stat = trim(code_cmmt).

return d_stat.

end.
~~~

### local-var09
    get_vehiculo (so__chr07)

funcion get_vehiculo
~~~
function get_vehiculo returns character(input ruta as char):
  define variable vehiculo as character.
  
  find first xxru-mstr no-lock where xxru-domain = global_domain
                                 and xxru-ruta   = ruta no-error.
  if avail xxru-mstr then vehiculo = xxru-vehiculo.
  return vehiculo.
end function.
~~~

### local-var07
    get_um_alterna(sod_part,pt_um,sod_um,sod_qty_ord)

### local-var08
    get_um_alterna(sod_part,pt_um,sod_um,sod_qty_ship)

Funcion get_um_alterna
~~~
function get_um_alterna return decimal(ipart as char,iptum as char,isodum as char,iqty as dec):

    define variable umconv_alt    like um_conv.

    /*Busco Unidad Medida alterna!!!!..... */
    umconv_alt = 1.
    find first um_mstr no-lock where um_domain = global_domain
                                 and um_part   = ipart
                                 and um_um     = iptum
                                 and um_alt_um = isodum no-error.
    if avail um_mstr then umconv_alt = um_conv.  

    return (iqty * umconv_alt).

end.
~~~

### local-var11
    (sod_qty_ord - sod_qty_inv) * sod_price


### local-var15
    get_desc_mod_aten (cm_db)

Funcion get_desc_mod_aten
~~~
FUNCTION get_desc_mod_aten RETURNS CHAR (imod AS CHAR):
    def var descri like code_cmmt.
    descri = "".
    find first code_mstr no-lock where code_domain  = global_domain
                                   and code_fldname = "cm_db"
                                   and code_value   = imod no-error.
    if avail code_mstr then descri = code_cmmt.
    RETURN descri.
END.
~~~

### local-var16
    descripcion_zona(cm_user1)

Funcion descripcion_zona
~~~
function descripcion_zona returns character(input codigo as char):
  define variable descri as character.
  find first code_mstr no-lock where code_domain  = global_domain 
                                 and code_fldname = "cm_mstr.cm_user1"
                                 and code_value   = codigo no-error.
                            
  if available code_mstr then descri = code_cmmt.
   else descri = "".
  return descri.
end function.
~~~
