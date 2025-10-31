

PREFIX = Dir(ARGUMENTS.get( "prefix", "/usr/local" )).abspath
print( "PREFIX: %s" % (PREFIX) )

SConscript( "XmlConfig/SConstruct", exports='PREFIX' )
SConscript( "RooPlotLib/SConstruct", exports='PREFIX' )
SConscript( "TaskEngine/SConstruct", exports='PREFIX' )
SConscript( "RootAna/SConstruct", exports='PREFIX' )

# Finally build a single complete library for people to use a single lib when they want everything
env = Environment()
target = env.StaticLibrary( target='RooBarbComplete', source=[ Glob( "**/*.o" ) ] )

# Install the library
install = env.Install( PREFIX + '/lib', [Glob("*.a")] )

Default(target, install)
