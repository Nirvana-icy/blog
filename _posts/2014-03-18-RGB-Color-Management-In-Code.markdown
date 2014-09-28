	
	
	static const char *CategoryColors =
	"208,138,138;"
	"217,183,141;"
	"228,230,144;"
	"189,230,144;"
	"170,229,144;"

	"171,229,186;"
	"170,229,229;"
	"153,184,228;"
	"139,139,227;"
	"151,122,198;"

	"208,138,226;"
	"208,139,182;"
	"208,138,140";

	@implementation DADataEnvironment

	SINGLETON_GCD(DADataEnvironment)

	+ (UIColor *)colorWithCategoryIndex:(NSUInteger)index{
    	NSString *colorString = [[NSString alloc] initWithBytes:CategoryColors length:strlen(CategoryColors) encoding:NSUTF8StringEncoding];
    
    	NSArray *array = [colorString componentsSeparatedByString:@";"];
    	NSString *string = [array objectAtIndex:MIN(array.count-1, index)];
    
    	NSArray *color = [string componentsSeparatedByString:@","];
    	return RGBCOLOR([color[0] integerValue], [color[1] integerValue], [color[2] integerValue]);
	}
	
