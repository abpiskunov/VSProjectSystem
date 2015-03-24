External customer guidance
==========================

CPS breaking changes in Dev14
-----------------------------


### Exports

Replace uses of:

    [PartMetadata(ProjectCapabilities.Requires, "…")]
    
With:

    [AppliesTo("…")]
    

If you have any class members that are themselves exported, you should
add the [AppliesTo] attribute to each of those members as well to match
the one that appears on the exported class (if any).


### Imports

Imports of IVsHierarchy, IVsProject, or IProjectConfigurationService must
be changed from:

    [Import(x)]
    
    Lazy<T> ImportingProperty { get; set; } 
    
To:

    [ImportMany(x)]
    
    OrderPrecedenceImportCollection<T> ImportingPropertyCollection { get;
    set; }
    
    
    Lazy<T> ImportingProperty
    
    {
    
        get { return this.ImportingPropertyCollection.First(); }
    
    } 
    
    
For any ImportMany of OrderPrecedenceImportCollection that you introduce,
you must add a line to your importing constructor:


    [ImportingConstructor]
    
    YourExtensionClassName(ConfiguredProject project) 
    
    {
    
         this.ImportedExtensions = new OrderPrecedenceImportCollection<T>(projectCapabilityCheckProvider:
     project);
    
     } 
    

### Changes that apply to only certain kinds of exports

#### IDynamicEnumValuesProvider 

Replace this pattern:

    [Export(typeof(IDynamicEnumValuesProvider))]
    
    [DynamicEnumCategory("…")] 
    
With:

    [ExportDynamicEnumValuesProvider("…")] 
    
