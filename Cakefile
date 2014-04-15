
fs = require 'fs'
{spawn, exec} = require 'child_process'

PLUGIN_COFFEE_DIR = "OpenEars/www/"

DEMO_COFFEE_DIR = "demo/www/js/"
DEMO_STYLUS_DIR = "demo/www/css/"


task "watch", "Watch & Compile Coffeescript & Stylus files", ->
    console.log "Spawning Coffee (Plugin)..."
    coffeeCompiler = spawn 'coffee', ['--compile', '--watch', '--output', PLUGIN_COFFEE_DIR, PLUGIN_COFFEE_DIR]
    coffeeCompiler.stdout.on 'data', (data) -> console.log data.toString().trim()
    coffeeCompiler.stderr.on 'data', (data) -> console.error data.toString().trim()

    console.log "Spawning Coffee (Demo)..."
    coffeeCompiler = spawn 'coffee', ['--compile', '--watch', '--output', DEMO_COFFEE_DIR, DEMO_COFFEE_DIR]
    coffeeCompiler.stdout.on 'data', (data) -> console.log data.toString().trim()
    coffeeCompiler.stderr.on 'data', (data) -> console.error data.toString().trim()

    console.log "Spawning Stylus..."
    stylusCompiler = spawn 'stylus', [DEMO_STYLUS_DIR, '--watch', '--out', DEMO_STYLUS_DIR]
    stylusCompiler.stdout.on 'data', (data) -> console.log data.toString().trim()
    stylusCompiler.stderr.on 'data', (data) -> console.error data.toString().trim()
